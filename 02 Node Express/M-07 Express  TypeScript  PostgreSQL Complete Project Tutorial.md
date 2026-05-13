# Express + TypeScript + PostgreSQL Complete Project Tutorial

এই প্রজেক্টে আমরা শিখবো:

* Express server setup
* TypeScript setup
* PostgreSQL connect
* CRUD API তৈরি
* Folder structure
* Environment variable
* Database table create
* API testing

---

# Final Project Structure

```txt
express/
│
├── node_modules/
│
├── src/
│   │
│   ├── config/
│   │   └── index.ts
│   │
│   └── server.ts
│
├── .env
├── package.json
├── tsconfig.json
└── package-lock.json
```

---

# Step 1: Project Folder Create

টার্মিনালে লিখো:

```bash
mkdir express
```

ফোল্ডারে ঢুকো:

```bash
cd express
```

---

# Step 2: npm Initialize

```bash
npm init -y
npm i -D typescript
npx tsc --init
npm install express --save
or npm i express

npm i --save -dev @types/express
npm i -D tsx
npm run dev 
```

এতে `package.json` তৈরি হবে।

tsconfig.json

"rootDir": "./src",
"outDir": "./dist",
"module": "esnext"
"target": "esnext";
"types": ["node"]
// "jsx" : "react-jsx" // comment it 

package.json

"type": "module"
"scripts":{
"dev": "tsx watch ./src/server.ts
}
---

# Step 3: Install Dependencies

## Main Dependencies

```bash
npm install express dotenv pg
```

### এগুলোর কাজ

| Package | কাজ                |
| ------- | ------------------ |
| express | Server তৈরি        |
| dotenv  | .env file read     |
| pg      | PostgreSQL connect |

---

## Dev Dependencies

```bash
npm install -D typescript tsx @types/express @types/pg @types/node
```

### এগুলোর কাজ

| Package    | কাজ                |
| ---------- | ------------------ |
| typescript | TypeScript support |
| tsx        | TypeScript run     |
| @types/*   | Type definition    |

---

# Step 4: TypeScript Initialize

```bash
npx tsc --init
```

এতে `tsconfig.json` তৈরি হবে।

---

# Step 5: Update package.json

তোমার `package.json`

```json
{
  "name": "express",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",

  "scripts": {
    "dev": "tsx watch ./src/server.ts",
    "test": "echo \"Error: no test specified\" && exit 1"
  },

  "keywords": [],
  "author": "",
  "license": "ISC",

  "type": "module",

  "devDependencies": {
    "@types/express": "^5.0.6",
    "@types/node": "^24.0.0",
    "@types/pg": "^8.20.0",
    "tsx": "^4.21.0",
    "typescript": "^6.0.3"
  },

  "dependencies": {
    "dotenv": "^17.4.2",
    "express": "^5.2.1",
    "pg": "^8.20.0"
  }
}
```

---

# Step 6: Update tsconfig.json

```json
{
  "compilerOptions": {
    "rootDir": "./src",
    "outDir": "./dist",

    "module": "esnext",
    "target": "esnext",
    "moduleResolution": "bundler",

    "types": ["node"],

    "sourceMap": true,

    "strict": true,

    "verbatimModuleSyntax": true,
    "isolatedModules": true,

    "skipLibCheck": true
  }
}
```

---

# Step 7: Create Folder Structure

```bash
mkdir src
mkdir src/config
```

---

# Step 8: Create .env File

Root folder এ `.env` file তৈরি করো।

```env
PORT=5000

CONNECTIONSTRING=postgresql://postgres:password@localhost:5432/express_db
```

---

# PostgreSQL Connection String Explanation

```txt
postgresql://username:password@localhost:5432/database_name
```

Example:

```txt
postgresql://postgres:1234@localhost:5432/express_db
```

| Part       | Meaning       |
| ---------- | ------------- |
| postgres   | username      |
| 1234       | password      |
| localhost  | server        |
| 5432       | postgres port |
| express_db | database name |

---

# Step 9: PostgreSQL Database Create

PostgreSQL shell বা pgAdmin এ যাও।

Database create করো:

```sql
CREATE DATABASE express_db;
```

---

# Step 10: Create Config File

## src/config/index.ts

```ts
import dotenv from "dotenv";
import path from "path";

dotenv.config({
  path: path.join(process.cwd(), ".env"),
});

const config = {
  connection_string: process.env.CONNECTIONSTRING as string,
  port: process.env.PORT,
};

export default config;
```

---

# Config File Explanation

| Code              | কাজ                 |
| ----------------- | ------------------- |
| dotenv.config()   | .env read করে       |
| process.env       | env variable access |
| connection_string | database URL        |
| port              | server port         |

---

# Step 11: Create Server File

## src/server.ts

```ts
import express, {
  type Application,
  type Request,
  type Response,
} from "express";

import { Pool } from "pg";

import config from "./config";

const app: Application = express();

const port = config.port;

/*
-----------------------------------
Middlewares
-----------------------------------
*/

app.use(express.json());
app.use(express.text());
app.use(express.urlencoded({ extended: true }));

/*
-----------------------------------
Database Connection
-----------------------------------
*/

const pool = new Pool({
  connectionString: config.connection_string,
});

/*
-----------------------------------
Create Table
-----------------------------------
*/

const initDB = async () => {
  try {
    await pool.query(`
      CREATE TABLE IF NOT EXISTS users(
        id SERIAL PRIMARY KEY,
        name VARCHAR(20),

        email VARCHAR(20) UNIQUE NOT NULL,

        password VARCHAR(20) NOT NULL,

        is_active BOOLEAN DEFAULT true,

        age INT,

        created_at TIMESTAMP DEFAULT NOW(),

        updated_at TIMESTAMP DEFAULT NOW()
      )
    `);

    console.log("Database connected successfully!");
  } catch (error) {
    console.log(error);
  }
};

initDB();

/*
-----------------------------------
Home Route
-----------------------------------
*/

app.get("/", (req: Request, res: Response) => {
  res.status(200).json({
    message: "Express Server",
    author: "Next Level",
  });
});

/*
-----------------------------------
Create User
-----------------------------------
*/

app.post("/api/users", async (req: Request, res: Response) => {
  const { name, email, password, age } = req.body;

  try {
    const result = await pool.query(
      `
      INSERT INTO users(name,email,password,age)
      VALUES($1,$2,$3,$4)
      RETURNING *
      `,
      [name, email, password, age],
    );

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
});

/*
-----------------------------------
Get All Users
-----------------------------------
*/

app.get("/api/users", async (req: Request, res: Response) => {
  try {
    const result = await pool.query(`
      SELECT * FROM users
    `);

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
});

/*
-----------------------------------
Get Single User
-----------------------------------
*/

app.get("/api/users/:id", async (req: Request, res: Response) => {
  const { id } = req.params;

  try {
    const result = await pool.query(
      `
      SELECT * FROM users
      WHERE id=$1
      `,
      [id],
    );

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
});

/*
-----------------------------------
Update User
-----------------------------------
*/

app.put("/api/users/:id", async (req: Request, res: Response) => {
  const { id } = req.params;

  const { name, password, age, is_active } = req.body;

  try {
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
      [name, password, age, is_active, id],
    );

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
});

/*
-----------------------------------
Delete User
-----------------------------------
*/

app.delete("/api/users/:id", async (req: Request, res: Response) => {
  const { id } = req.params;

  try {
    const result = await pool.query(
      `
      DELETE FROM users
      WHERE id=$1
      `,
      [id],
    );

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
});

/*
-----------------------------------
Server Listen
-----------------------------------
*/

app.listen(port, () => {
  console.log(`Server running on port ${port}`);
});
```

---

# Step 12: Run Project

```bash
npm run dev
```

Successful হলে দেখাবে:

```txt
Database connected successfully!
Server running on port 5000
```

---

# Step 13: Test API

## Home Route

### GET

```txt
http://localhost:5000/
```

---

# Create User

### POST

```txt
http://localhost:5000/api/users
```

Body:

```json
{
  "name": "Fahim",
  "email": "fahim@gmail.com",
  "password": "123456",
  "age": 22
}
```

---

# Get All Users

### GET

```txt
http://localhost:5000/api/users
```

---

# Get Single User

### GET

```txt
http://localhost:5000/api/users/1
```

---

# Update User

### PUT

```txt
http://localhost:5000/api/users/1
```

Body:

```json
{
  "name": "Updated Fahim",
  "age": 30
}
```

---

# Delete User

### DELETE

```txt
http://localhost:5000/api/users/1
```

---

# Important Concepts

| Concept     | Meaning               |
| ----------- | --------------------- |
| Middleware  | Request process করে   |
| Route       | API endpoint          |
| Pool        | PostgreSQL connection |
| async/await | asynchronous code     |
| req.body    | client data           |
| req.params  | URL parameter         |
| res.json()  | JSON response         |

---

# API Flow

```txt
Client
   ↓
Route
   ↓
Controller Logic
   ↓
Database Query
   ↓
Response
```

---

# Future Improvement

এই project পরে তুমি add করতে পারো:

* MVC architecture
* Separate routes
* Controllers
* Services
* JWT Authentication
* Password Hashing
* Validation
* Error Handler
* Prisma / Mongoose
* Docker
* Deployment

---

# Full Commands Summary

```bash
mkdir express

cd express

npm init -y

npm install express dotenv pg

npm install -D typescript tsx @types/express @types/pg @types/node

npx tsc --init

npm run dev
```
