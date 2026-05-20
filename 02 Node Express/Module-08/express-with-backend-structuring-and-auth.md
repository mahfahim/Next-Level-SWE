
---

# 📁 Proper Project Structure

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
│   │   │   ├── index.ts
│   │   │   
│   │   │
│   │   ├── modules/
│   │   │   └── user/
│   │   │       ├── user.controller.ts
│   │   │       ├── user.route.ts
│   │   │       ├── user.service.ts
│   │   │       └── user.interface.ts
│   │   │
│   │   
│   │       
│   │
│   ├── app.ts
│   └── server.ts
│
├── .env
├── package.json
└── tsconfig.json
```

---

# 📌 কোন ফাইলের কাজ কী?

| File                 | কাজ                       |
| -------------------- | ------------------------- |
| `server.ts`          | শুধু server start করবে    |
| `app.ts`             | express app configuration |
| `config/index.ts`    | env configuration         |
| `db/index.ts`        | database connection       |
| `db/initDB.ts`       | table create              |
| `user.route.ts`      | সব routes                 |
| `user.controller.ts` | request/response handle   |
| `user.service.ts`    | database query            |
| `user.interface.ts`  | TypeScript types          |

---

# 📌 1. `src/server.ts`

এখানে শুধু server start হবে।

```ts
import app from "./app.js";
import config from "./app/config/index.js";
import initDB from "./app/db/initDB.js";

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

এখানে express app setup হবে।

```ts
import express, { type Application } from "express";
import router from "./app/routes/index.js";

const app: Application = express();

app.use(express.json());
app.use(express.text());
app.use(express.urlencoded({ extended: true }));


app.use(router);

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
  connection_string: process.env.CONNECTIONSTRING as string,
  port: process.env.PORT,
  secret : process.env.JWT_SECRET
};

export default config;
```

---

# 📌 4. `src/app/db/index.ts`

Database connection এখানে থাকবে।

```ts
import { Pool } from "pg";
import config from "../config";

export const pool = new Pool({
  connectionString: config.connection_string,
});

export const initDB = async () => {
  try {
    await pool.query(`
        CREATE TABLE IF NOT EXISTS users(
        id SERIAL PRIMARY KEY,
        name VARCHAR(20),
        email VARCHAR(20) UNIQUE NOT NULL,
        password TEXT NOT NULL,
        is_active BOOLEAN DEFAULT true,
        age INT,

        created_at TIMESTAMP DEFAULT NOW(),
        updated_at TIMESTAMP DEFAULT NOW()
        )
            `);

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



---

# 📌 6. `src/app/modules/user/user.interface.ts`

Type define করার জন্য।

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

# 📌 7. `src/app/modules/user/user.service.ts`

সব database query এখানে থাকবে।

```ts
import bcrypt from "bcryptjs";
import { pool } from "../../db";
import type { IUser } from "./user.interface";
const createUserIntoDB = async (payload: IUser) => {
  const { name, email, password, age } = payload;

  const hashPassword = await bcrypt.hash(password, 10);

  const result = await pool.query(
    `
     INSERT INTO users(name,email,password,age) VALUES($1,$2,$3,$4) RETURNING *
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

const updateUserFromDB = async (payload: IUser, id: string) => {
  const { name, password, age, is_active } = payload;

  const result = await pool.query(
    `
    UPDATE users 
    SET 
    name=COALESCE($1,name),
    password=COALESCE($2,password),
    age=COALESCE($3,age),
    is_active=COALESCE($4,is_active) 

    WHERE id=$5 RETURNING *
    `,
    [name, password, age, is_active, id],
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

# 📌 8. `src/app/modules/user/user.controller.ts`

Controller request/response handle করে।

```ts
import type { Request, Response } from "express";
import { userService } from "./user.service";

const createUser = async (req: Request, res: Response) => {
  //   console.log(req.body);
  //   const { name, email, password, age } = req.body;

  try {
    const result = await userService.createUserIntoDB(req.body);
    // console.log(result);

    res.status(201).json({
      success: true,
      message: "User Created successfully!",
      data: result.rows[0],
    });
  } catch (error: any) {
    res.status(500).json({
      success: false,
      message: error.message,
      error: error,
    });
  }
};

const getAllUsers = async (req: Request, res: Response) => {
  try {
    const result = await userService.getAllUsersFromDB();
    res.status(200).json({
      success: true,
      message: "Users retrived successfully!",
      data: result.rows,
    });
  } catch (error: any) {
    res.status(500).json({
      success: false,
      message: error.message,
      error: error,
    });
  }
};

const getSingleUser = async (req: Request, res: Response) => {
  const { id } = req.params;
  try {
    const result = await userService.getSingleUserFromDB(id as string);
    if (result.rows.length === 0) {
      res.status(404).json({
        success: false,
        message: "User Not found!",
        data: {},
      });
    }

    res.status(200).json({
      success: true,
      message: "User retrived successfully!",
      data: result.rows[0],
    });
  } catch (error: any) {
    res.status(500).json({
      success: false,
      message: error.message,
      error: error,
    });
  }
};

const updateUser = async (req: Request, res: Response) => {
  const { id } = req.params;

  // console.log("Id : ", id);
  // console.log({ name, password, age, is_active });

  try {
    const result = await userService.updateUserFromDB(req.body, id as string);

    if (result.rows.length === 0) {
      res.status(404).json({
        success: false,
        message: "User Not found!",
      });
    }

    // console.log(result);
    res.status(200).json({
      success: true,
      message: "User updated successfully!",
      data: result.rows[0],
    });
  } catch (error: any) {
    res.status(500).json({
      success: false,
      message: error.message,
      error: error,
    });
  }
};

const deleteUser = async (req: Request, res: Response) => {
  const { id } = req.params;
  try {
    const result = await userService.deleteUserFromDB(id as string);

    console.log(result);
    if (result.rowCount === 0) {
      res.status(404).json({
        success: false,
        message: "User Not found!",
      });
    }

    res.status(200).json({
      success: true,
      message: "User deleted successfully!",
      data: {},
    });
  } catch (error: any) {
    res.status(500).json({
      success: false,
      message: error.message,
      error: error,
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

# 📌 9. `src/app/modules/user/user.route.ts`

সব user route এখানে থাকবে।

```ts
import { Router } from "express";
import { userController } from "./user.controller";

const router = Router();

router.post("/", userController.createUser);
router.get("/", userController.getAllUsers);
router.get("/:id", userController.getSingleUser);
router.put("/:id", userController.updateUser);
router.delete("/:id", userController.deleteUser);

export const userRoute = router;
```

---



---

# 🚀 এখন কী লাভ হলো?

আগে:

```txt
server.ts
├── route
├── database
├── query
├── controller
├── middleware
├── everything
```

এখন:

```txt
controller -> request handle
service -> database query
route -> route
db -> database
config -> env
```

---

# 📌 Industry তে এভাবে কেন করে?

কারণ:

### ✅ Maintain করা সহজ

১০ হাজার লাইনের file হয় না।

---

### ✅ Team work সহজ

একজন route এ কাজ করবে
একজন service এ
একজন controller এ

conflict কম হবে।

---

### ✅ Reusable

Service অন্য জায়গা থেকেও use করা যায়।

---

### ✅ Debugging সহজ

Problem কোথায় হয়েছে দ্রুত বুঝা যায়।

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

# 📌 Controller vs Service

| Controller          | Service             |
| ------------------- | ------------------- |
| req/res handle করে  | database query করে  |
| status code পাঠায়   | business logic রাখে |
| response return করে | data process করে    |

---

# 📌 Next Level Improvement

এরপর তুমি এগুলো শিখতে পারো:

* validation (zod/joi)
* error handler middleware
* async try catch wrapper
* authentication JWT
* bcrypt password hash
* prisma ORM
* mongoose
* clean architecture
* repository pattern
* MVC architecture
* modular architecture
