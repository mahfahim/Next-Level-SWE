# Simple Node.js + TypeScript CRUD Project Tutorial

এই project এ আমরা pure Node.js `http` module ব্যবহার করে একটা simple Product API বানাবো। এখানে কোনো Express.js ব্যবহার করা হয়নি।

তুমি শিখবে:

* Node.js server তৈরি
* Routing
* Controller
* Service layer
* JSON database
* GET / POST / PUT / DELETE API
* TypeScript setup
* `.env` config ব্যবহার

---

# Project Structure

```txt
learning_node/
│
├── src/
│   ├── config/
│   │   └── index.ts
│   │
│   ├── controller/
│   │   └── product.controller.ts
│   │
│   ├── database/
│   │   └── db.json
│   │
│   ├── routes/
│   │   └── route.ts
│   │
│   ├── service/
│   │   └── product.service.ts
│   │
│   ├── types/
│   │   └── product.type.ts
│   │
│   ├── utility/
│   │   ├── parseBody.ts
│   │   └── sendResponse.ts
│   │
│   └── server.ts
│
├── .env
├── package.json
├── tsconfig.json
└── nodemon.json
```

---

# Step 1: Project Initialize

টার্মিনালে রান করো:

```bash
mkdir learning_node        # learning_node নামে নতুন folder তৈরি করে
cd learning_node           # ওই folder এর ভিতরে ঢুকে যায়
npm init -y                # Node.js project initialize করে, package.json auto তৈরি হয়
```

---

# Step 2: Install Packages

```bash
npm install dotenv   # .env file থেকে environment variables (API key, DB password etc.) load করার জন্য dotenv package install করা হয়

npm install -D typescript ts-node-dev @types/node
# -D মানে devDependencies এ install হবে (শুধু development এর জন্য)

# typescript → TypeScript language support দেয় (compile এবং type checking এর জন্য)
# ts-node-dev → TypeScript code সরাসরি run করে এবং file change হলে auto restart করে
# @types/node → Node.js এর built-in features (fs, http etc.) TypeScript বুঝতে সাহায্য করে
```

---

# Step 3: TypeScript Initialize

```bash
npx tsc --init   # TypeScript project initialize করার জন্য tsconfig.json file তৈরি করে

# npx → কোনো package globally install না করেই run করার জন্য ব্যবহার হয়
# tsc → TypeScript compiler
# --init → TypeScript configuration file (tsconfig.json) auto generate করে

# tsconfig.json এর ভিতরে TypeScript কিভাবে compile হবে (target, module, strict mode etc.) সেটা control করা যায়
```

---

# Step 4: package.json Setup

```json
{
  "name": "learning_node",
  // প্রজেক্টের নাম (Node.js project identity)

  "version": "1.0.0",
  // প্রজেক্টের ভার্সন (initial release)

  "description": "",
  // প্রজেক্ট সম্পর্কে ছোট বিবরণ (এখন খালি রাখা হয়েছে)

  "main": "index.js",
  // entry point file (Node.js সাধারণত এখান থেকে start করে)
  // তবে TypeScript project এ সাধারণত src/server.ts ব্যবহার করা হয়

  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    // default test script
    // এখন কোন test setup নেই, তাই error message দেখাবে

    "dev": "ts-node-dev --respawn src/server.ts"
    // development mode এ server চালানোর command

    // ts-node-dev → TypeScript code সরাসরি run করে
    // --respawn → file change হলে server auto restart করে
    // src/server.ts → main server file
  },

  "keywords": [],
  // project search keyword (এখন খালি)

  "author": "",
  // project author নাম (এখন খালি)

  "license": "ISC",
  // open-source license type (ISC = simple permissive license)

  "type": "module",
  // Node.js কে ES Module হিসেবে ব্যবহার করতে বলে
  // অর্থাৎ import/export syntax ব্যবহার করা যাবে

  "dependencies": {
    "dotenv": "^17.4.2"
    // dotenv → .env file থেকে environment variables load করার জন্য
    // production + runtime এ দরকার হয়
  },

  "devDependencies": {
    "@types/node": "^25.7.0",
    // Node.js এর TypeScript type definitions
    // TypeScript কে Node.js features বুঝতে সাহায্য করে

    "ts-node-dev": "^2.0.0",
    // TypeScript run + auto restart tool (development এ use হয়)

    "typescript": "^6.0.3"
    // TypeScript compiler (TS → JS convert করার জন্য)
  }
}
```

---

# Step 5: Create `.env`

```env
PORT=5000   # এখানে PORT নামে একটি environment variable সেট করা হচ্ছে

# PORT → server কোন port number এ run করবে সেটা define করে
# 5000 → এই মানটা মানে server http://localhost:5000 এ চলবে

# সাধারণত এটা .env ফাইলে রাখা হয় যাতে code এর বাইরে config রাখা যায়
```

---

# Step 6: Config Setup

## `src/config/index.ts`

```ts
import dotenv from "dotenv";
// dotenv → একটি package যেটা .env file থেকে environment variables load করতে সাহায্য করে
// environment variables মানে: secret config (API key, PORT, DB password ইত্যাদি)

import path from "path";
// path → Node.js এর built-in module
// file path safely handle করার জন্য ব্যবহার হয় (Windows/Mac/Linux compatibility)

dotenv.config({ path: path.resolve(process.cwd(), ".env") });
// dotenv.config() → .env file কে load করে process.env এর ভিতরে নিয়ে আসে

// process.cwd() → current project root folder (যেখানে তুমি command চালাও)
// path.resolve(...) → full absolute path বানায়
// ".env" → environment file এর নাম

// 👉 মানে: নিশ্চিতভাবে project root এর .env file থেকে সব variables load করা হচ্ছে

const config = {
  port: Number(process.env.PORT),
  // config → একটা object যেখানে app এর settings রাখা হচ্ছে
  // const → variable reassign করা যাবে না
  // { } → object structure (key-value pair রাখার জন্য)

  // port → server কোন port এ run করবে সেটা define করে
  // process.env.PORT → .env file থেকে PORT value আনে

  // উদাহরণ:
  // .env → PORT="5000"
  // process.env.PORT → "5000" (string হিসেবে আসে)

  // Number(...) → string কে number এ convert করে
  // "5000" → 5000
  // কারণ server port number অবশ্যই number হওয়া উচিত
};

export default config;
// config → অন্য file গুলোতে ব্যবহার করার জন্য export করা হচ্ছে

// default export → import করার সময় { } লাগবে না
// উদাহরণ:
// import config from "./config";
```

## এখানে কী হচ্ছে?

* `dotenv` দিয়ে `.env` file read করা হচ্ছে
* `PORT` variable নেওয়া হচ্ছে
* পরে যেকোনো জায়গা থেকে config ব্যবহার করা যাবে

---

# Step 7: Create Server

## `src/server.ts`

```ts
import { createServer, IncomingMessage, Server, ServerResponse } from "http";
import config from "./config";
import { routeHandler } from "./routes/route";

const server: Server = createServer(
  (req: IncomingMessage, res: ServerResponse) => {
    routeHandler(req, res);
  },
);

server.listen(config.port, () => {
  console.log(`The server is running on the port ${config.port}`);
});
```

---

# Server Flow

```txt
Client Request
      ↓
server.ts
      ↓
routeHandler()
      ↓
Controller
      ↓
Service
      ↓
Database(JSON)
```

---

# Step 8: Database Setup

## `src/database/db.json`

```json
[
  {
    "id": 3,
    "name": "Portable Power Bank 20000mAh",
    "description": "Compact and powerful power bank",
    "price": 2500
  }
]
```

এখানে আমরা fake database হিসেবে JSON file ব্যবহার করছি।

---

# Step 9: Product Type

## `src/types/product.type.ts`

```ts
export interface IProduct {
  id: number;
  name: string;
  description: string;
  price: number;
}
```

## কেন Interface ব্যবহার করছি?

যাতে product object এর structure fixed থাকে।

---

# Step 10: Product Service

## `src/service/product.service.ts`

```ts
import fs from "fs";
import path from "path";

const filePath = path.join(process.cwd(), "./src/database/db.json");

export const readProduct = () => {
  const products = fs.readFileSync(filePath, "utf-8");

  return JSON.parse(products);
};

export const insertProduct = (payload: any) => {
  fs.writeFileSync(filePath, JSON.stringify(payload));
};
```

---

# এখানে কী হচ্ছে?

## `readProduct()`

JSON file থেকে data read করছে।

---

## `insertProduct()`

নতুন data JSON file এ save করছে।

---

# Step 11: Parse Request Body

## `src/utility/parseBody.ts`

```ts
import type { IncomingMessage } from "http";

export const parseBody = (req: IncomingMessage): Promise<any> => {
  return new Promise((resolve, reject) => {
    let body = "";

    req.on("data", (chunk) => {
      body += chunk;
    });

    req.on("end", () => {
      try {
        resolve(JSON.parse(body));
      } catch (error) {
        reject(error);
      }
    });
  });
};
```

---

# এটা কেন দরকার?

POST বা PUT request এ client body পাঠায়।

Example:

```json
{
  "name": "Keyboard",
  "price": 1200
}
```

এই body manually parse করতে হয় কারণ pure Node.js ব্যবহার করছি।

---

# Step 12: Send Response Utility

## `src/utility/sendResponse.ts`

```ts
import type { ServerResponse } from "http";

export const sendResponse = (
  res: ServerResponse,
  statusCode: number,
  success: boolean,
  message: string,
  data?: any,
) => {
  const response = {
    success,
    message,
    data,
  };

  res.writeHead(statusCode, {
    "content-type": "application/json",
  });

  res.end(JSON.stringify(response));
};
```

---

# সুবিধা কী?

বারবার:

```ts
res.writeHead()
res.end()
```

লিখতে হবে না।

---

# Step 13: Route Setup

## `src/routes/route.ts`

```ts
import type { IncomingMessage, ServerResponse } from "http";
import { productController } from "../controller/product.controller";

export const routeHandler = (
  req: IncomingMessage,
  res: ServerResponse,
) => {
  const url = req.url;
  const method = req.method;

  if (url === "/" && method === "GET") {
    res.writeHead(200, {
      "content-type": "application/json",
    });

    res.end(
      JSON.stringify({
        message: "This is root route",
      }),
    );
  } else if (url?.startsWith("/products")) {
    productController(req, res);
  } else {
    res.writeHead(404, {
      "content-type": "application/json",
    });

    res.end(
      JSON.stringify({
        message: "Route not found!",
      }),
    );
  }
};
```

---

# Route কাজ কী?

Route decide করে কোন request কোথায় যাবে।

Example:

| URL           | Method | Action         |
| ------------- | ------ | -------------- |
| `/`           | GET    | Root Route     |
| `/products`   | GET    | All Products   |
| `/products/1` | GET    | Single Product |

---

# Step 14: Product Controller

## `src/controller/product.controller.ts`

এটাই main logic।

এখানে:

* GET all products
* GET single product
* POST product
* PUT product
* DELETE product

সব handle করা হচ্ছে।

---

# URL Split Logic

```ts
const urlParts = url?.split("/");
```

যদি URL হয়:

```txt
/products/5
```

তাহলে:

```txt
['', 'products', '5']
```

---

# Product ID বের করা

```ts
const id =
  urlParts && urlParts[1] === "products"
    ? Number(urlParts[2])
    : null;
```

এখানে id হবে:

```txt
5
```

---

# GET All Products

```ts
if (url === "/products" && method === "GET")
```

সব product return করবে।

---

# GET Single Product

```ts
else if (method === "GET" && id !== null)
```

একটা specific product return করবে।

---

# POST Product

```ts
else if (method === "POST" && url === "/products")
```

নতুন product add করবে।

---

# PUT Product

```ts
else if (method === "PUT" && id !== null)
```

পুরানো product update করবে।

---

# DELETE Product

```ts
else if (method === "DELETE" && id !== null)
```

product delete করবে।

---

# Step 15: Run Project

```bash
npm run dev
```

Output:

```txt
The server is running on the port 5000
```

---

# API Testing

তুমি Postman বা Thunder Client ব্যবহার করতে পারো।

---

# 1. Get All Products

## Request

```http
GET /products
```

## Response

```json
{
  "success": true,
  "message": "Products retrived succeefully",
  "data": [
    {
      "id": 3,
      "name": "Portable Power Bank 20000mAh",
      "description": "Compact and powerful power bank",
      "price": 2500
    }
  ]
}
```

---

# 2. Create Product

## Request

```http
POST /products
```

## Body

```json
{
  "name": "Gaming Mouse",
  "description": "RGB Mouse",
  "price": 1500
}
```

---

# 3. Update Product

## Request

```http
PUT /products/3
```

## Body

```json
{
  "name": "Updated Mouse",
  "description": "New RGB Mouse",
  "price": 2000
}
```

---

# 4. Delete Product

## Request

```http
DELETE /products/3
```

---

# Important Concepts

| Concept       | Explanation               |
| ------------- | ------------------------- |
| Controller    | Request handle করে        |
| Service       | Database logic handle করে |
| Utility       | Reusable helper function  |
| Route         | URL manage করে            |
| Interface     | Data structure define করে |
| JSON Database | Fake database             |

---

# এই Project থেকে কী শিখবে?

* Backend fundamentals
* REST API
* HTTP methods
* File system
* TypeScript backend
* Node.js core module
* MVC pattern basics

---

# Future Improvement

তুমি চাইলে পরে add করতে পারো:

* Express.js
* MongoDB
* Prisma ORM
* Authentication
* JWT
* Validation
* Error Middleware
* Async File System
* UUID

---

# Final Note

এই project টা ছোট হলেও backend fundamentals শেখার জন্য অনেক গুরুত্বপূর্ণ।

কারণ এখানে তুমি framework ছাড়া raw Node.js দিয়ে বুঝতে পারবে:

* Request কীভাবে আসে
* Response কীভাবে যায়
* Route কীভাবে কাজ করে
* Body parse কীভাবে হয়
* CRUD internally কীভাবে কাজ করে

এগুলো ভালোভাবে বুঝতে পারলে পরে Express.js বা NestJS শেখা অনেক সহজ হয়ে যাবে।
