নিচে তোমার পুরো টিউটোরিয়ালটা **updated করে দিলাম, যেখানে Interface part টা একদম সহজ করে বুঝানো হয়েছে** এবং আগের মতোই console output রাখা হয়েছে।

---

# 📘 TypeScript Tutorial: Type Alias (01–10) + Console Output

Type Alias হলো TypeScript-এর একটি feature যেখানে আমরা কোনো type কে একটা নাম দিয়ে reuse করতে পারি। এটা code clean, readable এবং scalable করে।

---

## 01. Type Alias কী?

```ts id="1"
type UserName = string;

let name: UserName = "Fahim";

console.log(name);
```

### 🖥️ Output:

```
Fahim
```

---

## 02. Primitive Type Alias

```ts id="2"
type ID = number;
type IsActive = boolean;

let userId: ID = 101;
let active: IsActive = true;

console.log(userId);
console.log(active);
```

### 🖥️ Output:

```
101
true
```

---

## 03. Object Type Alias

```ts id="3"
type User = {
  id: number;
  name: string;
  age: number;
};

const user1: User = {
  id: 1,
  name: "Rahim",
  age: 25,
};

console.log(user1);
console.log(user1.name);
```

### 🖥️ Output:

```
{ id: 1, name: 'Rahim', age: 25 }
Rahim
```

---

## 04. Nested Object Alias

```ts id="4"
type Address = {
  city: string;
  country: string;
};

type User = {
  name: string;
  address: Address;
};

const user = {
  name: "Karim",
  address: {
    city: "Dhaka",
    country: "Bangladesh",
  },
};

console.log(user);
console.log(user.address.city);
```

### 🖥️ Output:

```
{ name: 'Karim', address: { city: 'Dhaka', country: 'Bangladesh' } }
Dhaka
```

---

## 05. Union Type Alias

```ts id="5"
type Status = "success" | "error" | "loading";

let apiStatus: Status = "success";

console.log(apiStatus);
```

### 🖥️ Output:

```
success
```

---

## 06. Function Type Alias

```ts id="6"
type Add = (x: number, y: number) => number;

const add: Add = (a, b) => a + b;

console.log(add(10, 20));
```

### 🖥️ Output:

```
30
```

---

## 07. Array Type Alias

```ts id="7"
type NumberList = number[];

const numbers: NumberList = [1, 2, 3, 4, 5];

console.log(numbers);
console.log(numbers[2]);
```

### 🖥️ Output:

```
[ 1, 2, 3, 4, 5 ]
3
```

---

## 08. Optional Properties

```ts id="8"
type User = {
  name: string;
  age?: number;
};

const user1: User = {
  name: "X",
};

console.log(user1);
console.log(user1.age);
```

### 🖥️ Output:

```
{ name: 'X' }
undefined
```

---

## 09. Type Alias vs Interface (Simple Version)

```ts id="9"
type User = {
  name: string;
};

// Interface হলো একই জিনিস, শুধু অন্য style

interface IUser {
  name: string;
}

const user = {
  name: "Fahim",
};

console.log(user);
```

### 🖥️ Output:

```
{ name: 'Fahim' }
```

---

## 🧠 সহজভাবে বোঝো (Important)

👉 `type` আর `interface` দুটোই একই কাজ করে
👉 শুধু difference হলো লিখার style

### 📌 Type:

```ts
type User = {
  name: string;
};
```

### 📌 Interface:

```ts
interface User {
  name: string;
}
```

---

## 🔥 সবচেয়ে important কথা:

👉 দুটোই শুধু **structure (shape)** define করে
👉 কোনো runtime effect নেই
👉 তাই console output একই থাকে

---

## 10. Real World Example (Your Code Style)

```ts id="10"
type Name = {
  firstName: string;
  lastName: string;
};

type Address = {
  division: string;
  city: string;
};

type User = {
  id: number;
  name: Name;
  gender: "male" | "female";
  contactNo: string;
  address: Address;
};

const user1: User = {
  id: 123,
  name: {
    firstName: "Mr.",
    lastName: "X",
  },
  gender: "male",
  contactNo: "0177",
  address: {
    division: "Chattogram",
    city: "Chattogram",
  },
};

const user2: User = {
  id: 124,
  name: {
    firstName: "Mr.",
    lastName: "Y",
  },
  gender: "female",
  contactNo: "01999",
  address: {
    division: "Dhaka",
    city: "Dhaka",
  },
};

const isAdmin: boolean = true;
const myName: string = "Me. X";

const add = (num1: number, num2: number): number => num1 + num2;

console.log(user1);
console.log(user2);
console.log(isAdmin);
console.log(myName);
console.log(add(5, 7));
```

### 🖥️ Output:

```
{
  id: 123,
  name: { firstName: 'Mr.', lastName: 'X' },
  gender: 'male',
  contactNo: '0177',
  address: { division: 'Chattogram', city: 'Chattogram' }
}
{
  id: 124,
  name: { firstName: 'Mr.', lastName: 'Y' },
  gender: 'female',
  contactNo: '01999',
  address: { division: 'Dhaka', city: 'Dhaka' }
}
true
Me. X
12
```

---

# 🔥 Final Summary

Type Alias তোমাকে help করে:

* clean code লিখতে
* reusable structure বানাতে
* complex data manage করতে
* real project সহজ করতে

---

# 🚀 Next Step

এখন এগুলো শিখো:

* Interface vs Type deep understanding
* Union & Intersection types
* Generics (very important)
* Utility Types (Pick, Omit, Partial)

---

চাওলে আমি next part বানিয়ে দিতে পারি:
👉 **“11–20 TypeScript Advanced + Real Project (API + CRUD + Type System)”**
