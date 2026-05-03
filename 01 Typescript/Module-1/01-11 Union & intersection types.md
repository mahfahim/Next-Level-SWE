নিচে আমি তোমার জন্য **“01–11 Union & Intersection Types in TypeScript” ফুল টিউটোরিয়াল** একদম সহজ বাংলায় + বাস্তব উদাহরণ + console output সহ সাজিয়ে দিলাম।

---

# 📘 TypeScript Tutorial: Union & Intersection Types (01–11)

TypeScript এ **Union (`|`)** এবং **Intersection (`&`)** খুব গুরুত্বপূর্ণ concept। এগুলো দিয়ে আমরা flexible এবং powerful type তৈরি করি।

---

# 🔹 01. Union Type কী?

Union মানে → **একাধিক টাইপের মধ্যে যেকোনো একটা**

```ts id="u1"
let role: "admin" | "user" | "guest";

role = "admin";
console.log(role);
```

### 🖥️ Output:

```txt id="ou1"
admin
```

---

# 🔹 02. Union Type with Function

```ts id="u2"
const getDashboard = (role: "admin" | "user" | "guest") => {
  if (role === "admin") {
    return "Admin Dashboard";
  } else if (role === "user") {
    return "User Dashboard";
  } else {
    return "Guest Dashboard";
  }
};

console.log(getDashboard("guest"));
```

### 🖥️ Output:

```txt id="ou2"
Guest Dashboard
```

---

# 🔹 03. Union Type with Variable

```ts id="u3"
let id: string | number;

id = 101;
console.log(id);

id = "A101";
console.log(id);
```

### 🖥️ Output:

```txt id="ou3"
101
A101
```

---

# 🔹 04. Union Type Real Example

```ts id="u4"
type Status = "success" | "error" | "loading";

let apiStatus: Status = "success";

console.log(apiStatus);
```

### 🖥️ Output:

```txt id="ou4"
success
```

---

# 🔹 05. Intersection Type কী?

Intersection মানে → **সব টাইপ একসাথে লাগবে**

```ts id="i1"
type Person = {
  name: string;
};

type Employee = {
  id: number;
};

type User = Person & Employee;

const user: User = {
  name: "Fahim",
  id: 1,
};

console.log(user);
```

### 🖥️ Output:

```txt id="oi1"
{ name: 'Fahim', id: 1 }
```

---

# 🔹 06. Intersection with Multiple Types

```ts id="i2"
type A = {
  a: string;
};

type B = {
  b: number;
};

type C = {
  c: boolean;
};

type ABC = A & B & C;

const obj: ABC = {
  a: "Hello",
  b: 10,
  c: true,
};

console.log(obj);
```

### 🖥️ Output:

```txt id="oi2"
{ a: 'Hello', b: 10, c: true }
```

---

# 🔹 07. Union vs Intersection Difference

```ts id="c1"
type A = { name: string };
type B = { age: number };

// Intersection
type Person = A & B;

const p: Person = {
  name: "Rahim",
  age: 25,
};

console.log(p);
```

### 🖥️ Output:

```txt id="oc1"
{ name: 'Rahim', age: 25 }
```

---

# 🔹 08. Real World Example (User Role System)

```ts id="r1"
type Admin = {
  role: "admin";
  permissions: string[];
};

type User = {
  role: "user";
  email: string;
};

type Account = Admin | User;

const user1: Account = {
  role: "admin",
  permissions: ["read", "write"],
};

console.log(user1);
```

### 🖥️ Output:

```txt id="or1"
{ role: 'admin', permissions: [ 'read', 'write' ] }
```

---

# 🔹 09. Function with Intersection Type

```ts id="f1"
type Name = {
  firstName: string;
  lastName: string;
};

type Info = {
  age: number;
};

type User = Name & Info;

const user: User = {
  firstName: "Md.",
  lastName: "Fahim",
  age: 22,
};

console.log(user.firstName);
```

### 🖥️ Output:

```txt id="of1"
Md.
```

---

# 🔹 10. Complex Real Example

```ts id="r2"
type Person = {
  id: string;
  name: string;
  phoneNo: string;
};

type Employee = {
  designation: string;
  teamSize: number;
};

type Manager = Person & Employee;

const manager: Manager = {
  id: "123",
  name: "Chowdhury Shaheb",
  phoneNo: "017",
  designation: "manager",
  teamSize: 20,
};

console.log(manager);
```

### 🖥️ Output:

```txt id="or2"
{
  id: '123',
  name: 'Chowdhury Shaheb',
  phoneNo: '017',
  designation: 'manager',
  teamSize: 20
}
```

---

# 🔹 11. Final Summary

## 🟡 Union (`|`)

👉 একাধিক option এর মধ্যে **একটা value**

```ts
type Role = "admin" | "user";
```

---

## 🟢 Intersection (`&`)

👉 সব type একসাথে combine করতে হয়

```ts
type User = Person & Employee;
```

---

# 🚀 সহজভাবে মনে রাখো

| Type             | মানে                |                         |
| ---------------- | ------------------- | ----------------------- |
| Union `          | `                   | একটার মধ্যে যেকোনো একটা |
| Intersection `&` | সবগুলো একসাথে লাগবে |                         |

---

# 🔥 Final Note

👉 Union = choice (OR logic)
👉 Intersection = combine (AND logic)

---

চাওলে আমি next বানিয়ে দিতে পারি:
👉 **“12–20 Advanced Type System + Real Project (Auth + API + Role System)”**

