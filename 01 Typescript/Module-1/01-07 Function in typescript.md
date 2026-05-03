ঠিক আছে, আমি তোমার টিউটোরিয়ালটা **আরও clean, correct এবং concept-clear করে update করে দিলাম**। এখানে সবচেয়ে গুরুত্বপূর্ণ জিনিস হলো `function type` vs `function` confusionও ঠিক করে দেওয়া হয়েছে 👇

---

# 🔰 01–07: Function in TypeScript (Full Tutorial Updated)

## 🧠 Function কী?

👉 Function হলো reusable block of code
👉 input নেয় → process করে → output দেয়

---

# 🔹 1. Basic Function

```ts
function add(num1: number, num2: number): number {
  return num1 + num2;
}
```

## 🧠 Breakdown

* `num1: number` → input type
* `: number` → return type

---

## ✅ Example

```ts
console.log(add(2, 3));
```

### 🟢 Output

```
5
```

---

# 🔹 2. Arrow Function

```ts
const addArrow = (num1: number, num2: number): number => num1 + num2;
```

👉 short modern syntax

---

## 🧠 Breakdown

* `(num1: number, num2: number)` → input
* `: number` → return type
* `=> num1 + num2` → logic

---

## ✅ Example

```ts
console.log(addArrow(4, 6));
```

### 🟢 Output

```
10
```

---

# 🔥 Important Concept: Function vs Function Type

## ❌ ভুল ধারণা

```ts
(x: number) => x
```

👉 এটা function না
👉 এটা function এর **type structure**

---

## ✅ Correct Function Type

```ts
(x: number) => number
```

👉 মানে:

> input number → output number

---

## 🔥 Real Function

```ts
const identity = (x: number) => x;
```

👉 এটা আসল function

---

# 🔹 3. Default Parameter

```ts
function greet(name: string = "Guest"): string {
  return "Hello " + name;
}
```

## ✅ Example

```ts
console.log(greet());
console.log(greet("Fahim"));
```

### 🟢 Output

```
Hello Guest
Hello Fahim
```

---

# 🔹 4. Optional Parameter

```ts
function fullName(first: string, last?: string): string {
  return last ? first + " " + last : first;
}
```

## ✅ Example

```ts
console.log(fullName("Fahim"));
console.log(fullName("Fahim", "Hai"));
```

### 🟢 Output

```
Fahim
Fahim Hai
```

---

# 🔹 5. Rest Parameter

```ts
function sum(...numbers: number[]): number {
  return numbers.reduce((acc, val) => acc + val, 0);
}
```

## ✅ Example

```ts
console.log(sum(1, 2, 3, 4));
```

### 🟢 Output

```
10
```

---

# 🔹 6. Object Method

```ts
const user = {
  name: "Fahim",
  balance: 0,
  addBalance(amount: number): number {
    this.balance += amount;
    return this.balance;
  },
};
```

## ✅ Example

```ts
console.log(user.addBalance(500));
```

### 🟢 Output

```
500
```

---

# 🔹 7. Callback Function

👉 function এর ভিতরে function

```ts
function process(n: number, cb: (x: number) => number) {
  return cb(n);
}
```

---

## 🧠 Breakdown

* `n` → number
* `cb` → function
* `(x: number) => number` → function type

---

## ✅ Example

```ts
const result = process(5, (x) => x * x);
console.log(result);
```

### 🟢 Output

```
25
```

---

# 🔹 8. Void Function

👉 কিছু return করে না

```ts
function logMessage(msg: string): void {
  console.log(msg);
}
```

---

# 🔹 9. Never Function

👉 function কখনো শেষ হয় না বা error দেয়

```ts
function throwError(msg: string): never {
  throw new Error(msg);
}
```

---

# 🔹 10. Function Type Alias

```ts
type AddType = (a: number, b: number) => number;

const addFunc: AddType = (a, b) => a + b;
```

---

# 🔹 11. Array Method (map)

```ts
const numbers = [1, 2, 3];

const squared = numbers.map((num: number) => num * num);
```

## 🟢 Output

```ts
console.log(squared);
```

```
[1, 4, 9]
```

---

# 🔹 12. Destructuring in Function

```ts
function getUser({ name, age }: { name: string; age: number }) {
  return name + " is " + age;
}
```

## ✅ Example

```ts
console.log(getUser({ name: "Fahim", age: 25 }));
```

### 🟢 Output

```
Fahim is 25
```

---

# 🔹 13. Return Object

```ts
function createUser(name: string, age: number) {
  return {
    name,
    age,
  };
}
```

---

# 🧪 Full Example

```ts
"use strict";

function add(a: number, b: number): number {
  return a + b;
}

const greet = (name: string = "Guest"): string => "Hello " + name;

function sum(...nums: number[]): number {
  return nums.reduce((acc, n) => acc + n, 0);
}

const user = {
  balance: 0,
  addBalance(amount: number): number {
    this.balance += amount;
    return this.balance;
  },
};

const process = (n: number, cb: (x: number) => number) => cb(n);

console.log(add(2, 3));
console.log(greet());
console.log(sum(1, 2, 3));
console.log(user.addBalance(100));
console.log(process(5, (x) => x * 2));
```

---

# 🟢 Final Output

```
5
Hello Guest
6
100
10
```

---

# 🔥 FINAL KEY TAKEAWAY

👉 Function = actual logic
👉 Function type = structure/shape
👉 `=> number` = type definition
👉 `=> x + y` = real function logic

---

চাও হলে আমি next এ তোমাকে **Type Alias vs Interface vs Function Type (interview level tricky)** একদম clear করে দিতে পারি 👍
