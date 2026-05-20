# 🚀 Updated Proper Express + TypeScript + PostgreSQL Project Structure

তোমার `auth`, `profile`, `user` module add করার পরে structure হবে এভাবে:

```txt
express/
│
├── src/
│   │
│   ├── app/
│   │   │
│   │   ├── config/
│   │   │   └── index.ts
│   │   │
│   │   ├── db/
│   │   │   └── index.ts
│   │   │
│   │   ├── routes/
│   │   │   └── index.ts
│   │   │
│   │   ├── modules/
│   │   │   │
│   │   │   ├── user/
│   │   │   │   ├── user.interface.ts
│   │   │   │   ├── user.service.ts
│   │   │   │   ├── user.controller.ts
│   │   │   │   └── user.route.ts
│   │   │   │
│   │   │   ├── auth/
│   │   │   │   ├── auth.service.ts
│   │   │   │   ├── auth.controller.ts
│   │   │   │   └── auth.route.ts
│   │   │   │
│   │   │   └── profile/
│   │   │       ├── profile.interface.ts
│   │   │       ├── profile.service.ts
│   │   │       ├── profile.controller.ts
│   │   │       └── profile.route.ts
│   │   │
│   │   └── middleware/
│   │
│   ├── app.ts
│   └── server.ts
│
├── .env
├── package.json
└── tsconfig.json
```

---

# 📌 Architecture Flow

```txt
Client Request
      ↓
Route
      ↓
Controller
      ↓
Service
      ↓
Database
```

---

# 📌 প্রতিটা Folder এর কাজ

| Folder/File | কাজ                      |
| ----------- | ------------------------ |
| config      | env config               |
| db          | database connection      |
| modules     | feature/module wise code |
| routes      | সব route combine         |
| middleware  | custom middleware        |
| app.ts      | express app config       |
| server.ts   | server start             |

---

# 📌 1. `src/server.ts`

শুধু server start হবে এখানে।

```ts
import app from "./app.js";
import config from "./app/config/index.js";
import { initDB } from "./app/db/index.js";

const startServer = async () => {
  try {
    await initDB();

    app.listen(config.port, () => {
      console.log(`Server running on port ${config.port}`);
    });
  } catch (error) {
    console.log(error);
  }
};

startServer();
```

---

# 📌 2. `src/app.ts`

Express app configuration এখানে থাকবে।

```ts
import express, { type Application } from "express";
import router from "./app/routes/index.js";

const app: Application = express();

app.use(express.json());
app.use(express.urlencoded({ extended: true }));
app.use(express.text());

app.use("/api/v1", router);

export default app;
```

---

# 📌 3. `src/app/config/index.ts`

```ts
import dotenv from "dotenv";
import path from "path";

dotenv.config({
  path: path.join(process.cwd(), ".env"),
});

const config = {
  port: process.env.PORT,
  connection_string: process.env.CONNECTIONSTRING as string,
  secret: process.env.JWT_SECRET as string,
};

export default config;
```

---

# 📌 4. `src/app/db/index.ts`

```ts
import { Pool } from "pg";
import config from "../config/index.js";

export const pool = new Pool({
  connectionString: config.connection_string,
});

export const initDB = async () => {
  try {
    // users table
    await pool.query(`
      CREATE TABLE IF NOT EXISTS users(
        id SERIAL PRIMARY KEY,
        name VARCHAR(50),
        email VARCHAR(50) UNIQUE NOT NULL,
        password TEXT NOT NULL,
        age INT,
        is_active BOOLEAN DEFAULT true,

        created_at TIMESTAMP DEFAULT NOW(),
        updated_at TIMESTAMP DEFAULT NOW()
      )
    `);

    // profiles table
    await pool.query(`
      CREATE TABLE IF NOT EXISTS profiles(
        id SERIAL PRIMARY KEY,

        user_id INT UNIQUE REFERENCES users(id) ON DELETE CASCADE,

        bio TEXT,
        address TEXT,
        phone VARCHAR(15),
        gender VARCHAR(10),

        created_at TIMESTAMP DEFAULT NOW(),
        updated_at TIMESTAMP DEFAULT NOW()
      )
    `);

    console.log("Database connected successfully!");
  } catch (error) {
    console.log(error);
  }
};
```

---

# 📌 5. `src/app/routes/index.ts`

সব module route এখানে combine হবে।

```ts
import { Router } from "express";

import { userRoute } from "../modules/user/user.route.js";
import { authRoute } from "../modules/auth/auth.route.js";
import { profileRoute } from "../modules/profile/profile.route.js";

const router = Router();

router.use("/users", userRoute);
router.use("/auth", authRoute);
router.use("/profiles", profileRoute);

export default router;
```

---

# 👤 USER MODULE

# 📌 6. `user.interface.ts`

```ts
export interface IUser {
  name: string;
  email: string;
  password: string;
  age: number;
  is_active?: boolean;
}
```

---

# 📌 7. `user.service.ts`

Database query এখানে থাকবে।

```ts
import bcrypt from "bcryptjs";
import { pool } from "../../db/index.js";
import type { IUser } from "./user.interface.js";

const createUserIntoDB = async (payload: IUser) => {
  const { name, email, password, age } = payload;

  const hashPassword = await bcrypt.hash(password, 10);

  const result = await pool.query(
    `
    INSERT INTO users(name,email,password,age)
    VALUES($1,$2,$3,$4)
    RETURNING *
    `,
    [name, email, hashPassword, age],
  );

  delete result.rows[0].password;

  return result;
};

const getAllUsersFromDB = async () => {
  const result = await pool.query(`
    SELECT * FROM users
  `);

  return result;
};

const getSingleUserFromDB = async (id: string) => {
  const result = await pool.query(
    `
    SELECT * FROM users WHERE id=$1
    `,
    [id],
  );

  return result;
};

const updateUserFromDB = async (
  payload: Partial<IUser>,
  id: string,
) => {
  const { name, password, age, is_active } = payload;

  let hashPassword = null;

  if (password) {
    hashPassword = await bcrypt.hash(password, 10);
  }

  const result = await pool.query(
    `
    UPDATE users
    SET
      name=COALESCE($1,name),
      password=COALESCE($2,password),
      age=COALESCE($3,age),
      is_active=COALESCE($4,is_active)

    WHERE id=$5
    RETURNING *
    `,
    [name, hashPassword, age, is_active, id],
  );

  return result;
};

const deleteUserFromDB = async (id: string) => {
  const result = await pool.query(
    `
    DELETE FROM users WHERE id=$1
    `,
    [id],
  );

  return result;
};

export const userService = {
  createUserIntoDB,
  getAllUsersFromDB,
  getSingleUserFromDB,
  updateUserFromDB,
  deleteUserFromDB,
};
```

---

# 📌 8. `user.controller.ts`

```ts
import type { Request, Response } from "express";
import { userService } from "./user.service.js";

const createUser = async (req: Request, res: Response) => {
  try {
    const result = await userService.createUserIntoDB(req.body);

    res.status(201).json({
      success: true,
      message: "User created successfully!",
      data: result.rows[0],
    });
  } catch (error: any) {
    res.status(500).json({
      success: false,
      message: error.message,
    });
  }
};

const getAllUsers = async (req: Request, res: Response) => {
  try {
    const result = await userService.getAllUsersFromDB();

    res.status(200).json({
      success: true,
      message: "Users retrieved successfully!",
      data: result.rows,
    });
  } catch (error: any) {
    res.status(500).json({
      success: false,
      message: error.message,
    });
  }
};

const getSingleUser = async (req: Request, res: Response) => {
  const { id } = req.params;

  try {
    const result = await userService.getSingleUserFromDB(id);

    if (result.rows.length === 0) {
      return res.status(404).json({
        success: false,
        message: "User not found!",
      });
    }

    res.status(200).json({
      success: true,
      message: "User retrieved successfully!",
      data: result.rows[0],
    });
  } catch (error: any) {
    res.status(500).json({
      success: false,
      message: error.message,
    });
  }
};

const updateUser = async (req: Request, res: Response) => {
  const { id } = req.params;

  try {
    const result = await userService.updateUserFromDB(req.body, id);

    if (result.rows.length === 0) {
      return res.status(404).json({
        success: false,
        message: "User not found!",
      });
    }

    res.status(200).json({
      success: true,
      message: "User updated successfully!",
      data: result.rows[0],
    });
  } catch (error: any) {
    res.status(500).json({
      success: false,
      message: error.message,
    });
  }
};

const deleteUser = async (req: Request, res: Response) => {
  const { id } = req.params;

  try {
    const result = await userService.deleteUserFromDB(id);

    if (result.rowCount === 0) {
      return res.status(404).json({
        success: false,
        message: "User not found!",
      });
    }

    res.status(200).json({
      success: true,
      message: "User deleted successfully!",
    });
  } catch (error: any) {
    res.status(500).json({
      success: false,
      message: error.message,
    });
  }
};

export const userController = {
  createUser,
  getAllUsers,
  getSingleUser,
  updateUser,
  deleteUser,
};
```

---

# 📌 9. `user.route.ts`

```ts
import { Router } from "express";
import { userController } from "./user.controller.js";

const router = Router();

router.post("/", userController.createUser);
router.get("/", userController.getAllUsers);
router.get("/:id", userController.getSingleUser);
router.put("/:id", userController.updateUser);
router.delete("/:id", userController.deleteUser);

export const userRoute = router;
```

---

# 🔐 AUTH MODULE

# 📌 10. `auth.service.ts`

```ts
import bcrypt from "bcryptjs";
import jwt from "jsonwebtoken";

import { pool } from "../../db/index.js";
import config from "../../config/index.js";

const loginUserIntoDB = async (payload: any) => {
  const { email, password } = payload;

  // check user exists
  const user = await pool.query(
    `
    SELECT * FROM users WHERE email=$1
    `,
    [email],
  );

  if (user.rows.length === 0) {
    throw new Error("User not found!");
  }

  const isPasswordMatched = await bcrypt.compare(
    password,
    user.rows[0].password,
  );

  if (!isPasswordMatched) {
    throw new Error("Password does not match!");
  }

  const jwtPayload = {
    id: user.rows[0].id,
    email: user.rows[0].email,
  };

  const token = jwt.sign(jwtPayload, config.secret, {
    expiresIn: "7d",
  });

  delete user.rows[0].password;

  return {
    token,
    user: user.rows[0],
  };
};

export const authService = {
  loginUserIntoDB,
};
```

---

# 📌 11. `auth.controller.ts`

```ts
import type { Request, Response } from "express";
import { authService } from "./auth.service.js";

const loginUser = async (req: Request, res: Response) => {
  try {
    const result = await authService.loginUserIntoDB(req.body);

    res.status(200).json({
      success: true,
      message: "User login successful!",
      data: result,
    });
  } catch (error: any) {
    res.status(500).json({
      success: false,
      message: error.message,
    });
  }
};

export const authController = {
  loginUser,
};
```

---

# 📌 12. `auth.route.ts`

```ts
import { Router } from "express";
import { authController } from "./auth.controller.js";

const router = Router();

router.post("/login", authController.loginUser);

export const authRoute = router;
```

---

# 👤 PROFILE MODULE

# 📌 13. `profile.interface.ts`

```ts
export interface IProfile {
  user_id: number;
  bio?: string;
  address?: string;
  phone?: string;
  gender?: string;
}
```

---

# 📌 14. `profile.service.ts`

```ts
import { pool } from "../../db/index.js";
import type { IProfile } from "./profile.interface.js";

const createProfileIntoDB = async (payload: IProfile) => {
  const { user_id, bio, address, phone, gender } = payload;

  // check user exists
  const user = await pool.query(
    `
    SELECT * FROM users WHERE id=$1
    `,
    [user_id],
  );

  if (user.rows.length === 0) {
    throw new Error("User does not exist!");
  }

  const result = await pool.query(
    `
    INSERT INTO profiles(user_id,bio,address,phone,gender)
    VALUES($1,$2,$3,$4,$5)
    RETURNING *
    `,
    [user_id, bio, address, phone, gender],
  );

  return result;
};

export const profileService = {
  createProfileIntoDB,
};
```

---

# 📌 15. `profile.controller.ts`

```ts
import type { Request, Response } from "express";
import { profileService } from "./profile.service.js";

const createProfile = async (req: Request, res: Response) => {
  try {
    const result = await profileService.createProfileIntoDB(req.body);

    res.status(201).json({
      success: true,
      message: "Profile created successfully!",
      data: result.rows[0],
    });
  } catch (error: any) {
    res.status(500).json({
      success: false,
      message: error.message,
    });
  }
};

export const profileController = {
  createProfile,
};
```

---

# 📌 16. `profile.route.ts`

```ts
import { Router } from "express";
import { profileController } from "./profile.controller.js";

const router = Router();

router.post("/", profileController.createProfile);

export const profileRoute = router;
```

---

# 📌 API URL Example

## User

```txt
POST   /api/v1/users
GET    /api/v1/users
GET    /api/v1/users/:id
PUT    /api/v1/users/:id
DELETE /api/v1/users/:id
```

---

## Auth

```txt
POST /api/v1/auth/login
```

---

## Profile

```txt
POST /api/v1/profiles
```

---

# 📌 কেন Modular Structure Industry Standard?

## ✅ Code clean থাকে

সব code এক file এ থাকে না।

---

## ✅ Large project manageable হয়

১০০+ route handle করা সহজ হয়।

---

## ✅ Team work easy হয়

একজন auth module
একজন user module
একজন profile module

আলাদা কাজ করতে পারে।

---

## ✅ Reusable হয়

service অন্য জায়গা থেকেও reuse করা যায়।

---

## ✅ Testing easy হয়

প্রতিটা module আলাদা test করা যায়।

---

# 📌 Next Level Improvement

এখন এগুলো শিখো:

* validation using zod
* custom error handler
* async wrapper
* JWT middleware
* role based auth
* refresh token
* Prisma ORM
* Mongoose
* Clean Architecture
* Repository Pattern
* MVC Architecture
* Modular Architecture
* Global error handler
* Environment validation
* Cookie based auth
* Access & Refresh token system
