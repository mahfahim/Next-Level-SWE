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
npx tsc --init

// npx tsc --init কমান্ডটা TypeScript প্রজেক্টে একটি tsconfig.json ফাইল তৈরি করে, যেটা দিয়ে TypeScript compiler-এর সেটিংস কনফিগার করা হয়।
```

---

# Step 4: package.json Setup

```json
{
  // প্রজেক্টের নাম
  "name": "learning_node",

  // প্রজেক্টের ভার্সন
  "version": "1.0.0",

  // প্রজেক্টের ছোট বিবরণ (এখন খালি)
  "description": "",

  // এন্ট্রি ফাইল (পুরনো Node.js স্টাইল, এখন অনেক সময় ব্যবহার হয় না TypeScript এ)
  "main": "index.js",

  "scripts": {
    
    // dev command: development mode এ server চালায়
    // tsx ব্যবহার করে TypeScript সরাসরি run করে
    // watch mode মানে কোড change হলে auto restart হবে
    "dev": "tsx watch ./src/server.ts",

    // test command (default auto generated)
    // এখানে এখন কোন test setup করা নেই
    "test": "echo \"Error: no test specified\" && exit 1"
  },

  // এখন খালি array (future এ keyword use করা যায়)
  "keywords": [],

  // author এর নাম (এখন খালি)
  "author": "",

  // লাইসেন্স টাইপ (open source permission টাইপ)
  "license": "ISC",

  // শুধু development এর জন্য দরকারি packages
  "devDependencies": {

    // Node.js এর TypeScript type definitions
    // এগুলো না থাকলে Node টাইপ error দিতে পারে
    "@types/node": "^25.5.0",

    // TypeScript compiler
    "typescript": "^6.0.2"
  },

  // runtime এ যেগুলো দরকার (production এ লাগে)
  "dependencies": {

    // environment variable manage করার জন্য (.env file)
    "dotenv": "^17.3.1",

    // TypeScript run করার tool
    // ts-node এর modern + fast version
    "tsx": "^4.21.0"
  }
}
```

---

# Step 5: Create `.env`

```env
PORT=5000
```

---

# Step 6: Config Setup

## `src/config/index.ts`

```ts
import dotenv from "dotenv";
import path from "path";

dotenv.config({ path: path.resolve(process.cwd(), ".env") });

const config = {
  port: Number(process.env.PORT),
};

export default config;
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
