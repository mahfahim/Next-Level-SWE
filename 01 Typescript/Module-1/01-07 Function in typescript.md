ঠিক আছে, এবার **Function in TypeScript** একদম শুরু থেকে advanced পর্যন্ত full tutorial দিচ্ছি—clear example + output সহ 👇

---

# 🔰 01–07: Function in TypeScript (Full Tutorial)

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

👉 short version

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

# 🔥 Normal vs Arrow

| বিষয়   | Normal  | Arrow |
| ------ | ------- | ----- |
| syntax | বড়      | ছোট   |
| `this` | dynamic | fixed |

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

👉 unknown number of input

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

# 🔹 6. Function in Object (Method)

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

# 🔥 Important: `this`

👉 `this` = current object

---

# 🔹 7. Callback Function

👉 function এর ভিতরে function

```ts
function process(num: number, callback: (x: number) => number) {
  return callback(num);
}
```

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

👉 function শেষ হয় না

```ts
function throwError(msg: string): never {
  throw new Error(msg);
}
```

---

# 🔹 10. Function Type Alias

👉 function এর type আলাদা করে define

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

---

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

# 🔥 Quick Revision

* function → input + output
* arrow → short syntax
* optional → `?`
* default → value দেওয়া
* rest → `...`
* callback → function inside function
* method → object function
* void → return নাই
* never → শেষ হয় না

---

# 🚀 Pro Tips

* সবসময় return type লিখলে code safe হয়
* callback ভালো বুঝলে async concept সহজ হবে
* arrow function বেশি use হয় modern code এ

---

চাও হলে আমি তোমাকে **real project style function usage (API call, async/await, error handling)** দেখাতে পারি 👍
