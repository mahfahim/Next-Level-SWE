

# 🚀 Express + TypeScript + PostgreSQL (Modern Setup & CRUD Tutorial)

এই টিউটোরিয়ালে আমরা একদম শুরু থেকে modern setup (ESM & NodeNext) অনুযায়ী একটি Express + TypeScript + PostgreSQL প্রজেক্ট রান করবো এবং সম্পূর্ণ CRUD API তৈরি করবো।

---

## Step 0: আগে কী কী Install করতে হবে

### ১. Node.js

Node.js Official Website থেকে ইন্সটল করো।
Install করার পর টার্মিনালে চেক করো:

```bash
node -v
npm -v

```

### ২. PostgreSQL

PostgreSQL Official Website থেকে ইন্সটল করো।

* Install করার সময় মনে রাখবে:
* **username:** `postgres`
* **password:** তোমার দেওয়া পাসওয়ার্ড (যেমন: `1234`)
* **port:** `5432`



### ৩. VS Code & Extensions

Visual Studio Code ইন্সটল করো এবং নিচের এক্সটেনশনগুলো অ্যাড করে নাও:

| Extension | কাজ |
| --- | --- |
| **ES7+ React Snippets** | Shortcut এর জন্য |
| **Error Lens** | Error highlight করার জন্য |
| **Prettier** | Code formatting এর জন্য |
| **PostgreSQL** | Database manage করার জন্য |
| **Thunder Client** | API test করার জন্য (Postman এর বিকল্প) |

---

## Step 1: Project Initialize & Package Install

টার্মিনাল ওপেন করে নিচের কমান্ডগুলো ধাপে ধাপে রান করো:

```bash
# ১. ফোল্ডার তৈরি ও প্রবেশ
mkdir express
cd express

# ২. npm Initialize (package.json তৈরি হবে)
npm init -y

# ৩. Main Packages Install
npm install express dotenv pg

# ৪. Dev Packages Install (TypeScript & TSX)
npm install -D typescript tsx @types/node @types/express @types/pg

# ৫. TypeScript Initialize (tsconfig.json তৈরি হবে)
npx tsc --init

# ৬. ফোল্ডার স্ট্রাকচার তৈরি
mkdir src
mkdir src/config

```

---

## Step 2: Configuration Files Setup

প্রজেক্টের রুট ফোল্ডারে থাকা ফাইলগুলো আপডেট করে নাও।

### 1. `package.json`

`"type": "module"` এবং `"dev"` স্ক্রিপ্ট অ্যাড করে পুরো ফাইলটি এরকম করো:

```json

{
  "name": "express",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "dev": "tsx watch ./src/server.ts", ///////////////////////////////////////////////// vvi
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "type": "module", ///////////////////////////////////////////////// vvi
  "devDependencies": {
    "@types/express": "^5.0.6",
    "@types/pg": "^8.20.0",
    "typescript": "^6.0.3"
  },
  "dependencies": {
    "dotenv": "^17.4.2",
    "express": "^5.2.1",
    "pg": "^8.20.0",
    "tsx": "^4.21.0"
  }
}


```

### 2. `tsconfig.json`

পুরো ফাইলটি রিপ্লেস করে দাও। এখানে `"module": "NodeNext"` ব্যবহার করা হয়েছে যা Modern Node.js ESM সিস্টেমকে দারুণভাবে সাপোর্ট করে।

```json


{
  // Visit https://aka.ms/tsconfig to read more about this file
  "compilerOptions": {
    // File Layout
    "rootDir": "./src", //////////////////// vvi
    "outDir": "./dist", //////////////////// vvi
    // Environment Settings
    // See also https://aka.ms/tsconfig/module
    "module": "esnext", //////////////////// vvi
    "target": "esnext", /////////////////// vvi
    "moduleResolution": "bundler", /////////////////////// vvi
    "types": ["node"], ////////////////////////////// vvi
    // For nodejs:
    // "lib": ["esnext"],
    // "types": ["node"],
    // and npm install -D @types/node

    // Other Outputs
    "sourceMap": true,
    "declaration": true,
    "declarationMap": true,

    // Stricter Typechecking Options
    "noUncheckedIndexedAccess": true,
    "exactOptionalPropertyTypes": true,

    // Style Options
    // "noImplicitReturns": true,
    // "noImplicitOverride": true,
    // "noUnusedLocals": true,
    // "noUnusedParameters": true,
    // "noFallthroughCasesInSwitch": true,
    // "noPropertyAccessFromIndexSignature": true,

    // Recommended Options
    "strict": true,
    // "jsx": "react-jsx",  ///////////////////// vvi, just comment it
    "verbatimModuleSyntax": true,
    "isolatedModules": true,
    "noUncheckedSideEffectImports": true,
    "moduleDetection": "force",
    "skipLibCheck": true
  }
}


```

---

## Step 3: Initial Server Setup & Test (খুবই গুরুত্বপূর্ণ)

ফুল প্রজেক্ট লেখার আগে আমরা দেখবো আমাদের সার্ভার ঠিকমতো রান হচ্ছে কিনা।

### 1. `src/server.ts` এ বেসিক কোড লেখো:

```typescript
import  express , {type Request , type Response, } from "express";

const app = express();
const port = 5000;

app.get("/",(req:Request, res:Response)=>{
    res.send("Initial server is running successfully");
});

app.listen(port, ()=>{
    console.log(`Server is running on port ${port}`);
})

```

### 2. প্রজেক্ট রান করো:

টার্মিনালে লিখো:

```bash
npm run dev

```

**Output আসবে:** `Server is running on port 5000`

ব্রাউজারে গিয়ে `http://localhost:5000` লিখে এন্টার দাও। যদি `Initial Server is Running Successfully! 🎉` লেখা দেখতে পাও, তার মানে তোমার সেটআপ ১০০% সাকসেসফুল!

*(সার্ভার রান থাকা অবস্থাতেই তুমি এখন পরবর্তী কাজগুলো করতে পারো, `tsx` এর কারণে ফাইল সেভ করলেই অটো রিস্টার্ট হবে)*

---

## Step 4: Database & Environment Setup

### 1. Database Create (pgAdmin)

PostgreSQL এর **pgAdmin** ওপেন করো। Query Tool এ গিয়ে নিচের কোডটি রান করে ডেটাবেস তৈরি করো:

```sql
CREATE DATABASE express_db;

```

### 2. `.env` File

প্রজেক্টের রুটে (যেখানে package.json আছে) `.env` নামে একটি ফাইল তৈরি করো এবং নিচের কোডগুলো দাও (পাসওয়ার্ড তোমার অনুযায়ী পরিবর্তন করে নিও):

```env
PORT=5000
CONNECTIONSTRING=postgresql://postgres:1234@localhost:5432/express_db

```

### 3. `src/config/index.ts`

Environment ভ্যারিয়েবলগুলো লোড করার জন্য এই ফাইলটি তৈরি করো:

```typescript
import dotenv from "dotenv";
import path from "path";

dotenv.config({
  path: path.join(process.cwd(), ".env"),
});

const config = {
  port: process.env.PORT,
  connectionString: process.env.CONNECTIONSTRING,
};

export default config;

```

---

## Step 5: Full Application Code (CRUD Implementation)

এবার তোমার আগের বেসিক `src/server.ts` ফাইলটি মুছে ফেলে নিচের সম্পূর্ণ কোডটি দিয়ে দাও। এখানে ডেটাবেস কানেকশন, টেবিল তৈরি এবং সব CRUD API একসাথে দেওয়া হলো।
*(লক্ষ্য করো: NodeNext মডিউল ব্যবহার করায় ইম্পোর্টের সময় `.js` এক্সটেনশন ব্যবহার করা হয়েছে)*

### `src/server.ts`

```typescript
// express package থেকে express function এবং কিছু type import করা হচ্ছে
import express, {
  type Application, // পুরো Express app এর type
  type Request, // request object এর type
  type Response, // response object এর type
} from "express";

// PostgreSQL database এর সাথে connect করার জন্য Pool import করা হচ্ছে
import { Pool } from "pg";

// config file থেকে configuration import করা হচ্ছে
import config from "./config";


// express() function call করে app তৈরি করা হচ্ছে
const app: Application = express();

// config থেকে port নেওয়া হচ্ছে
const port = config.port;



// =========================
// Middleware Section
// =========================

// JSON data receive করার জন্য middleware
app.use(express.json());

// plain text receive করার জন্য middleware
app.use(express.text());

// form data handle করার জন্য middleware
app.use(express.urlencoded({ extended: true }));



// =========================
// Database Connection
// =========================

// PostgreSQL connection pool তৈরি
const pool = new Pool({
  // database connection string
  connectionString: config.connection_string,
});



// =========================
// Database Initialization
// =========================

// async function তৈরি করা হয়েছে database table create করার জন্য
const initDB = async () => {
  try {

    // SQL query run করা হচ্ছে
    await pool.query(`
        
        // users table তৈরি হবে যদি আগে না থাকে
        CREATE TABLE IF NOT EXISTS users(

        // auto increment id
        id SERIAL PRIMARY KEY,

        // name column
        name VARCHAR(20),

        // unique email
        email VARCHAR(20) UNIQUE NOT NULL,

        // password required
        password VARCHAR(20) NOT NULL,

        // active status
        is_active BOOLEAN DEFAULT true,

        // age column
        age INT,

        // automatically created time
        created_at TIMESTAMP DEFAULT NOW(),

        // automatically updated time
        updated_at TIMESTAMP DEFAULT NOW()
        )
    `);

    // success message
    console.log("Database connected successfully!");

  } catch (error) {

    // error হলে console এ দেখাবে
    console.log(error);
  }
};

// function call করা হচ্ছে
initDB();



// =========================
// Home Route
// =========================

// GET request handle করবে "/"
app.get("/", (req: Request, res: Response) => {

  // JSON response পাঠানো হচ্ছে
  res.status(200).json({

    // message
    message: "Express Server",

    // author name
    author: "Next Level",
  });
});



// =========================
// Create User API
// =========================

// POST request দিয়ে user create করা হবে
app.post("/api/users", async (req: Request, res: Response) => {

  // request body থেকে data নেওয়া হচ্ছে
  const { name, email, password, age } = req.body;

  try {

    // database এ insert query চালানো হচ্ছে
    const result = await pool.query(

      `
      INSERT INTO users(name,email,password,age)
      VALUES($1,$2,$3,$4)
      RETURNING *
      `,

      // values array
      [name, email, password, age],
    );

    // success response
    res.status(201).json({
      success: true,
      message: "User Created successfully!",

      // inserted row return করবে
      data: result.rows[0],
    });

  } catch (error: any) {

    // error response
    res.status(500).json({
      success: false,
      message: error.message,
      error: error,
    });
  }
});



// =========================
// Get All Users
// =========================

// সব user পাওয়ার route
app.get("/api/users", async (req: Request, res: Response) => {

  try {

    // সব user select করা হচ্ছে
    const result = await pool.query(`
      SELECT * FROM users
    `);

    // response পাঠানো হচ্ছে
    res.status(200).json({
      success: true,
      message: "Users retrived successfully!",

      // সব rows
      data: result.rows,
    });

  } catch (error: any) {

    // error response
    res.status(500).json({
      success: false,
      message: error.message,
      error: error,
    });
  }
});



// =========================
// Get Single User
// =========================

// dynamic route parameter :id
app.get("/api/users/:id", async (req: Request, res: Response) => {

  // URL parameter থেকে id নেওয়া হচ্ছে
  const { id } = req.params;

  try {

    // নির্দিষ্ট user select করা হচ্ছে
    const result = await pool.query(
      `
      SELECT * FROM users WHERE id=$1
      `,
      [id],
    );

    // user না পেলে
    if (result.rows.length === 0) {

      res.status(404).json({
        success: false,
        message: "User Not found!",
        data: {},
      });
    }

    // user পাওয়া গেলে
    res.status(200).json({
      success: true,
      message: "User retrived successfully!",

      // single user
      data: result.rows[0],
    });

  } catch (error: any) {

    // error response
    res.status(500).json({
      success: false,
      message: error.message,
      error: error,
    });
  }
});



// =========================
// Update User
// =========================

// PUT request দিয়ে update করা হবে
app.put("/api/users/:id", async (req: Request, res: Response) => {

  // params থেকে id
  const { id } = req.params;

  // body থেকে data
  const { name, password, age, is_active } = req.body;

  try {

    // UPDATE query
    const result = await pool.query(
      `
      UPDATE users

      SET

      // নতুন value না দিলে পুরানো value থাকবে
      name = COALESCE($1,name),

      password = COALESCE($2,password),

      age = COALESCE($3,age),

      is_active = COALESCE($4,is_active)

      WHERE id=$5

      RETURNING *
      `,

      // values array
      [name, password, age, is_active, id],
    );

    // user না পেলে
    if (result.rows.length === 0) {

      res.status(404).json({
        success: false,
        message: "User Not found!",
      });
    }

    // success response
    res.status(200).json({
      success: true,
      message: "User updated successfully!",

      // updated user
      data: result.rows[0],
    });

  } catch (error: any) {

    // error response
    res.status(500).json({
      success: false,
      message: error.message,
      error: error,
    });
  }
});



// =========================
// Delete User
// =========================

// DELETE request
app.delete("/api/users/:id", async (req: Request, res: Response) => {

  // id নেওয়া হচ্ছে
  const { id } = req.params;

  try {

    // delete query
    const result = await pool.query(
      `
      DELETE FROM users WHERE id=$1
      `,
      [id],
    );

    // console log
    console.log(result);

    // user না থাকলে
    if (result.rowCount === 0) {

      res.status(404).json({
        success: false,
        message: "User Not found!",
      });
    }

    // success response
    res.status(200).json({
      success: true,
      message: "User deleted successfully!",

      // empty object
      data: {},
    });

  } catch (error: any) {

    // error response
    res.status(500).json({
      success: false,
      message: error.message,
      error: error,
    });
  }
});



// =========================
// Start Server
// =========================

// server start করা হচ্ছে
app.listen(port, () => {

  // console message
  console.log(`Example app listening on port ${port}`);
});

```

---

## Step 6: Final Run & Checking

ফাইল সেভ করার সাথে সাথেই টার্মিনালে অটোমেটিক আপডেট হয়ে এই মেসেজগুলো দেখাবে (যদি সার্ভার আগে থেকে রান থাকে, নাহলে `npm run dev` দিয়ে স্টার্ট করো):

```txt
Server running on 5000
Database Connected!
Users table created!

```

---

## 🎯 API Reference & Testing

Thunder Client বা Postman ব্যবহার করে নিচের রাউটগুলো টেস্ট করতে পারো:

| Method | Route | কাজ |
| --- | --- | --- |
| **GET** | `/` | Home Route (সার্ভার চেক) |
| **POST** | `/api/users` | নতুন User Create করা |
| **GET** | `/api/users` | সব User দেখা |
| **GET** | `/api/users/:id` | নির্দিষ্ট একজন User দেখা |
| **PUT** | `/api/users/:id` | নির্দিষ্ট User আপডেট করা |
| **DELETE** | `/api/users/:id` | নির্দিষ্ট User ডিলিট করা |

### Test Example (POST Create User)

**URL:** `http://localhost:5000/api/users`

**Body (JSON):**

```json
{
  "name": "Fahim",
  "email": "fahim@gmail.com",
  "password": "123456",
  "age": 22
}

```

---

### 🎉 Final Learning & Recap

অভিনন্দন! তুমি সফলভাবে শিখে ফেলেছো:

* Express + TypeScript Setup
* NodeNext / ESM Setup
* **Initial Server Testing** (ভুল কমানোর জন্য)
* Environment Variables (`.env`)
* PostgreSQL Connection (`pg` pool)
* Database Queries (`INSERT`, `SELECT`, `UPDATE`, `DELETE`)
* COALESCE ফাংশনের মাধ্যমে ডায়নামিক আপডেট
* Full REST API (CRUD basics)

  ---
  ---
  ---
  ---
  ---

  তুমি একদম ঠিক বলছো।
`সব কিছু server.ts এ লিখলে` project বড় হওয়ার সাথে সাথে manage করা কঠিন হয়ে যায়।

Industry standard অনুযায়ী আলাদা আলাদা file/folder এ ভাগ করা হয়।

নিচে আমি তোমার project টা proper structure এ organize করে দিলাম।

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
│   │   │   └── initDB.ts
│   │   │
│   │   ├── modules/
│   │   │   └── user/
│   │   │       ├── user.controller.ts
│   │   │       ├── user.route.ts
│   │   │       ├── user.service.ts
│   │   │       └── user.interface.ts
│   │   │
│   │   └── routes/
│   │       └── index.ts
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
  port: process.env.PORT,
  connectionString: process.env.CONNECTIONSTRING,
};

export default config;
```

---

# 📌 4. `src/app/db/index.ts`

Database connection এখানে থাকবে।

```ts
import { Pool } from "pg";
import config from "../config/index.js";

const pool = new Pool({
  connectionString: config.connectionString,
});

export default pool;
```

---

# 📌 5. `src/app/db/initDB.ts`

Table create এখানে হবে।

```ts
import pool from "./index.js";

const initDB = async () => {
  try {
    await pool.query(`
    
      CREATE TABLE IF NOT EXISTS users(
      
        id SERIAL PRIMARY KEY,
        name VARCHAR(50),
        email VARCHAR(50) UNIQUE NOT NULL,
        password VARCHAR(50) NOT NULL,
        age INT,
        is_active BOOLEAN DEFAULT true,
        created_at TIMESTAMP DEFAULT NOW(),
        updated_at TIMESTAMP DEFAULT NOW()
      )
    
    `);

    console.log("Database Connected!");
    console.log("Users table created!");
  } catch (error) {
    console.log(error);
  }
};

export default initDB;
```

---

# 📌 6. `src/app/modules/user/user.interface.ts`

Type define করার জন্য।

```ts
export interface IUser {
  name: string;
  email: string;
  password: string;
  age: number;
}
```

---

# 📌 7. `src/app/modules/user/user.service.ts`

সব database query এখানে থাকবে।

```ts
import pool from "../../db/index.js";
import { IUser } from "./user.interface.js";



const createUserIntoDB = async (payload: IUser) => {
  const { name, email, password, age } = payload;

  const result = await pool.query(
    `
      INSERT INTO users(name,email,password,age)
      VALUES($1,$2,$3,$4)
      RETURNING *
    `,
    [name, email, password, age]
  );

  return result.rows[0];
};




const getAllUsersFromDB = async () => {
  const result = await pool.query(`
    SELECT * FROM users
  `);

  return result.rows;
};




const getSingleUserFromDB = async (id: string) => {
  const result = await pool.query(
    `
      SELECT * FROM users
      WHERE id=$1
    `,
    [id]
  );

  return result.rows[0];
};




const updateUserIntoDB = async (
  id: string,
  payload: Partial<IUser>
) => {
  const { name, password, age } = payload;

  const result = await pool.query(
    `
      UPDATE users
      
      SET
      name = COALESCE($1,name),
      password = COALESCE($2,password),
      age = COALESCE($3,age)

      WHERE id=$4

      RETURNING *
    `,
    [name, password, age, id]
  );

  return result.rows[0];
};




const deleteUserFromDB = async (id: string) => {
  const result = await pool.query(
    `
      DELETE FROM users
      WHERE id=$1
    `,
    [id]
  );

  return result.rowCount;
};




export const UserServices = {
  createUserIntoDB,
  getAllUsersFromDB,
  getSingleUserFromDB,
  updateUserIntoDB,
  deleteUserFromDB,
};
```

---

# 📌 8. `src/app/modules/user/user.controller.ts`

Controller request/response handle করে।

```ts
import { type Request, type Response } from "express";
import { UserServices } from "./user.service.js";



const createUser = async (req: Request, res: Response) => {
  try {
    const result = await UserServices.createUserIntoDB(req.body);

    res.status(201).json({
      success: true,
      message: "User created successfully!",
      data: result,
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
    const result = await UserServices.getAllUsersFromDB();

    res.status(200).json({
      success: true,
      data: result,
    });
  } catch (error: any) {
    res.status(500).json({
      success: false,
      message: error.message,
    });
  }
};




const getSingleUser = async (req: Request, res: Response) => {
  try {
    const result = await UserServices.getSingleUserFromDB(
      req.params.id
    );

    res.status(200).json({
      success: true,
      data: result,
    });
  } catch (error: any) {
    res.status(500).json({
      success: false,
      message: error.message,
    });
  }
};




const updateUser = async (req: Request, res: Response) => {
  try {
    const result = await UserServices.updateUserIntoDB(
      req.params.id,
      req.body
    );

    res.status(200).json({
      success: true,
      message: "User updated successfully!",
      data: result,
    });
  } catch (error: any) {
    res.status(500).json({
      success: false,
      message: error.message,
    });
  }
};




const deleteUser = async (req: Request, res: Response) => {
  try {
    await UserServices.deleteUserFromDB(req.params.id);

    res.status(200).json({
      success: true,
      message: "User deleted successfully!",
      data: {},
    });
  } catch (error: any) {
    res.status(500).json({
      success: false,
      message: error.message,
    });
  }
};




export const UserControllers = {
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
import express from "express";
import { UserControllers } from "./user.controller.js";

const router = express.Router();

router.post("/users", UserControllers.createUser);

router.get("/users", UserControllers.getAllUsers);

router.get("/users/:id", UserControllers.getSingleUser);

router.put("/users/:id", UserControllers.updateUser);

router.delete("/users/:id", UserControllers.deleteUser);

export default router;
```

---

# 📌 10. `src/app/routes/index.ts`

সব module route এক জায়গায় collect হবে।

```ts
import express from "express";
import userRoutes from "../modules/user/user.route.js";

const router = express.Router();

router.get("/", (req, res) => {
  res.json({
    success: true,
    message: "Server running successfully!",
  });
});

router.use("/api", userRoutes);

export default router;
```

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

