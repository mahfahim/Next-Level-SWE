ঠিক আছে, এবার **Non-Primitive Types (TypeScript)** নিয়ে একদম complete tutorial দিচ্ছি—array, tuple, object সব clear হবে 👇

---

# 🔰 01–05: Explore Non-Primitive Types (Full Tutorial)

## 🧠 Non-Primitive Type কী?

👉 যেগুলো multiple value বা structure ধরে রাখে:

* Array
* Tuple (TS special)
* Object

---

# 🔹 1. Array

## 👉 Basic Array

```ts
let bazarList: string[] = ["eggs", "milk", "sugar"];
```

👉 এখানে:

* শুধু string থাকবে

---

## ❌ Wrong Example

```ts
bazarList.push(true); // ❌
```

👉 error কারণ:

* array = string[]
* তুমি boolean ঢুকাচ্ছো

---

## ✅ Correct Example

```ts
bazarList.push("rice"); // ✔
```

---

## 🔹 Number Array

```ts
let numbers: number[] = [1, 2, 3];
```

---

## 🔹 Mixed Array (Union Type)

```ts
let mixedArr: (string | number)[] = ["eggs", 12, "milk", 1];
```

👉 এখানে:

* string + number allowed

---

## 🟢 Example + Output

```ts
let items: (string | number)[] = ["apple", 10];
items.push("banana");

console.log(items);
```

### Output:

```json
["apple", 10, "banana"]
```

---

# 🔹 2. Tuple (TypeScript Special)

👉 Tuple = fixed type + fixed position

---

## ✅ Basic Tuple

```ts
let couple: [string, string] = ["Husband", "Wife"];
```

👉 index:

* 0 → string
* 1 → string

---

## ✅ Another Example

```ts
let destination: [string, string, number] = ["Dhaka", "Chattogram", 3];
```

👉 মানে:

* city1 → string
* city2 → string
* distance → number

---

## ❌ Wrong

```ts
destination[0] = 100; // ❌
```

👉 কারণ index 0 = string হওয়া লাগবে

---

## ⚠️ Important (JS behavior)

```ts
destination.push("extra"); // JS এ allow 😐
```

👉 TypeScript এটা পুরোপুরি prevent করতে পারে না runtime এ

---

## 🟢 Example + Output

```ts
let userInfo: [string, number] = ["Fahim", 25];

console.log(userInfo);
```

### Output:

```json
["Fahim", 25]
```

---

# 🔹 3. Object (Most Important)

## 👉 Basic Object

```ts
const user = {
  firstName: "Jhankar",
  lastName: "Mahbub",
  isMarried: true,
};
```

---

## ✅ TypeScript Typed Object

```ts
const user: {
  firstName: string;
  lastName: string;
  isMarried: boolean;
} = {
  firstName: "Jhankar",
  lastName: "Mahbub",
  isMarried: true,
};
```

---

# 🔥 Advanced Concepts

---

## 🔹 1. Literal Type

```ts
const user: {
  organization: "Programming Hero";
} = {
  organization: "Programming Hero",
};
```

👉 এখানে value fixed

---

## ❌ Wrong

```ts
user.organization = "PH"; // ❌
```

---

## 🔹 2. Optional Property

```ts
const user: {
  firstName: string;
  middleName?: string;
  lastName: string;
} = {
  firstName: "Jhankar",
  lastName: "Mahbub",
};
```

👉 `middleName` না দিলেও চলবে

---

## 🔹 3. Readonly Property

```ts
const user: {
  readonly id: number;
  name: string;
} = {
  id: 1,
  name: "Fahim",
};

// user.id = 2; ❌
```

---

# 🔹 Real Example (Full)

```ts
"use strict";

let bazarList: string[] = ["eggs", "milk", "sugar"];
bazarList.push("rice");

let mixedArr: (string | number)[] = ["eggs", 12, "milk"];

let couple: [string, string] = ["Husband", "Wife"];

let destination: [string, string, number] = ["Dhaka", "Chattogram", 3];

const user: {
  organization: "Programming Hero";
  firstName: string;
  lastName: string;
  isMarried: boolean;
} = {
  organization: "Programming Hero",
  firstName: "Jhankar",
  lastName: "Mahbub",
  isMarried: true,
};

// user.organization = "Other"; ❌

console.log(bazarList);
console.log(mixedArr);
console.log(couple);
console.log(destination);
console.log(user);
```

---

# 🟢 Final Output

```json
["eggs", "milk", "sugar", "rice"]
["eggs", 12, "milk"]
["Husband", "Wife"]
["Dhaka", "Chattogram", 3]
{
  "organization": "Programming Hero",
  "firstName": "Jhankar",
  "lastName": "Mahbub",
  "isMarried": true
}
```

---

# 🔥 Quick Revision

## Array

👉 same type list
`string[]`

## Tuple

👉 fixed structure
`[string, number]`

## Object

👉 key-value structure
strong typing possible

---

# 🚀 Pro Tips

* সবসময় type explicitly define করলে bug কম হয়
* tuple use করো fixed data এর জন্য
* object এ optional + readonly use করলে code safe হয়

---

চাও হলে আমি next step হিসেবে **Function Type, Type Alias, Interface** একদম roadmap আকারে বুঝিয়ে দিতে পারি 👍

