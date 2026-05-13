

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
  "type": "module",
  "scripts": {
    "dev": "tsx watch ./src/server.ts"
  },
  "dependencies": {
    "dotenv": "^17.4.2",
    "express": "^5.2.1",
    "pg": "^8.20.0"
  },
  "devDependencies": {
    "@types/express": "^5.0.6",
    "@types/node": "^24.0.0",
    "@types/pg": "^8.20.0",
    "tsx": "^4.21.0",
    "typescript": "^6.0.3"
  }
}

```

### 2. `tsconfig.json`

পুরো ফাইলটি রিপ্লেস করে দাও। এখানে `"module": "NodeNext"` ব্যবহার করা হয়েছে যা Modern Node.js ESM সিস্টেমকে দারুণভাবে সাপোর্ট করে।

```json
{
  "compilerOptions": {
    "rootDir": "./src",
    "outDir": "./dist",
    "module": "NodeNext",
    "target": "ESNext",
    "moduleResolution": "NodeNext",
    "types": ["node"],
    "sourceMap": true,
    "strict": true,
    "skipLibCheck": true
  },
  "include": ["src"]
}

```

---

## Step 3: Initial Server Setup & Test (খুবই গুরুত্বপূর্ণ)

ফুল প্রজেক্ট লেখার আগে আমরা দেখবো আমাদের সার্ভার ঠিকমতো রান হচ্ছে কিনা।

### 1. `src/server.ts` এ বেসিক কোড লেখো:

```typescript
import express from "express";

const app = express();
const port = 5000;

app.get("/", (req, res) => {
  res.send("Initial Server is Running Successfully! 🎉");
});

app.listen(port, () => {
  console.log(`Server is running on port ${port}`);
});

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
import express from "express";
import { Pool } from "pg";
import config from "./config/index.js"; // 👈 ESM system expects .js

const app = express();
const port = config.port || 5000;

/* --------------------------------
   Middlewares
-------------------------------- */
app.use(express.json());
app.use(express.text());
app.use(express.urlencoded({ extended: true }));

/* --------------------------------
   Database Connection
-------------------------------- */
const pool = new Pool({
  connectionString: config.connectionString,
});

/* --------------------------------
   Test Database & Init Table
-------------------------------- */
const initDB = async () => {
  try {
    await pool.query("SELECT NOW()");
    console.log("Database Connected!");

    await pool.query(`
      CREATE TABLE IF NOT EXISTS users(
        id SERIAL PRIMARY KEY,
        name VARCHAR(50),
        email VARCHAR(100) UNIQUE NOT NULL,
        password VARCHAR(100) NOT NULL,
        age INT,
        is_active BOOLEAN DEFAULT true,
        created_at TIMESTAMP DEFAULT NOW()
      )
    `);
    console.log("Users table created!");
  } catch (error) {
    console.log(error);
  }
};

initDB();

/* --------------------------------
   Routes
-------------------------------- */

// 1. Home Route
app.get("/", (req, res) => {
  res.status(200).json({
    success: true,
    message: "Express PostgreSQL Server Running...",
  });
});

// 2. Create User (POST)
app.post("/api/users", async (req, res) => {
  const { name, email, password, age } = req.body;
  try {
    const result = await pool.query(
      `INSERT INTO users(name,email,password,age)
       VALUES($1,$2,$3,$4) RETURNING *`,
      [name, email, password, age],
    );
    res.status(201).json({
      success: true,
      message: "User created successfully!",
      data: result.rows[0],
    });
  } catch (error: any) {
    res.status(500).json({ success: false, message: error.message });
  }
});

// 3. Get All Users (GET)
app.get("/api/users", async (req, res) => {
  try {
    const result = await pool.query(`SELECT * FROM users`);
    res.status(200).json({
      success: true,
      message: "Users retrieved successfully!",
      data: result.rows,
    });
  } catch (error: any) {
    res.status(500).json({ success: false, message: error.message });
  }
});

// 4. Get Single User (GET)
app.get("/api/users/:id", async (req, res) => {
  const { id } = req.params;
  try {
    const result = await pool.query(`SELECT * FROM users WHERE id=$1`, [id]);
    if (result.rows.length === 0) {
      return res.status(404).json({ success: false, message: "User not found!" });
    }
    res.status(200).json({
      success: true,
      message: "User retrieved successfully!",
      data: result.rows[0],
    });
  } catch (error: any) {
    res.status(500).json({ success: false, message: error.message });
  }
});

// 5. Update User (PUT)
app.put("/api/users/:id", async (req, res) => {
  const { id } = req.params;
  const { name, password, age, is_active } = req.body;
  try {
    const result = await pool.query(
      `UPDATE users
       SET name=COALESCE($1,name), password=COALESCE($2,password), age=COALESCE($3,age), is_active=COALESCE($4,is_active)
       WHERE id=$5 RETURNING *`,
      [name, password, age, is_active, id],
    );
    if (result.rows.length === 0) {
      return res.status(404).json({ success: false, message: "User not found!" });
    }
    res.status(200).json({
      success: true,
      message: "User updated successfully!",
      data: result.rows[0],
    });
  } catch (error: any) {
    res.status(500).json({ success: false, message: error.message });
  }
});

// 6. Delete User (DELETE)
app.delete("/api/users/:id", async (req, res) => {
  const { id } = req.params;
  try {
    const result = await pool.query(`DELETE FROM users WHERE id=$1`, [id]);
    if (result.rowCount === 0) {
      return res.status(404).json({ success: false, message: "User not found!" });
    }
    res.status(200).json({
      success: true,
      message: "User deleted successfully!",
    });
  } catch (error: any) {
    res.status(500).json({ success: false, message: error.message });
  }
});

/* --------------------------------
   Start Server
-------------------------------- */
app.listen(port, () => {
  console.log(`Server running on ${port}`);
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
