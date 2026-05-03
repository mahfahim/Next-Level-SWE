ঠিক আছে, আমি তোমাকে একদম শুরু থেকে **Primitive Types (TypeScript)** নিয়ে একটা complete tutorial দিচ্ছি—concept + code + example + output সহ। এমনভাবে সাজানো যেন একবার পড়লেই clear হয়ে যায়।

---

# 🔰 01–04: Explore Primitive Types (Full Tutorial)

## 🧠 Primitive Type কী?

Primitive type মানে হলো basic data type—যেগুলো directly value store করে।

👉 TypeScript এ primitive types গুলো:

* `string`
* `number`
* `boolean`
* `undefined`
* `null`
* `void`
* `never`
* `unknown`

---

# 🔹 1. string

👉 Text বা লেখা রাখার জন্য

```ts
let userName: string = "mezba123";
```

### ✅ Example

```ts
let firstName: string = "Fahim";
let lastName: string = "Hai";

let fullName = firstName + " " + lastName;

console.log(fullName);
```

### 🟢 Output:

```
Fahim Hai
```

---

## ❌ Common Mistake

```ts
let userName: string = "mezba123";
userName.toFixed(); // ❌ ভুল
```

👉 কারণ `toFixed()` number এর method

---

# 🔹 2. number

👉 সব ধরনের সংখ্যা (int, float)

```ts
let userId: number = 123;
let price: number = 99.5;
```

### ✅ Example

```ts
let num: number = 123;

console.log(num.toFixed(2));
```

### 🟢 Output:

```
123.00
```

---

# 🔹 3. boolean

👉 true / false

```ts
let isAdmin: boolean = false;
```

### ✅ Example

```ts
let isLoggedIn: boolean = true;

if (isLoggedIn) {
  console.log("User logged in");
}
```

### 🟢 Output:

```
User logged in
```

---

# 🔹 4. undefined

👉 variable declare করা আছে, কিন্তু value নাই

```ts
let x: undefined = undefined;
```

### ✅ Example

```ts
let data;

console.log(data);
```

### 🟢 Output:

```
undefined
```

---

# 🔹 5. null

👉 intentionally empty value

```ts
let y: null = null;
```

### ✅ Example

```ts
let user = null;

console.log(user);
```

### 🟢 Output:

```
null
```

---

# 🔹 6. void

👉 function কিছু return করবে না

```ts
function logMessage(): void {
  console.log("Hello");
}
```

### 🟢 Output:

```
Hello
```

---

# 🔹 7. never

👉 function কখনো শেষ হয় না / error throw করে

### ✅ Example 1 (infinite loop)

```ts
function loop(): never {
  while (true) {}
}
```

### ✅ Example 2 (error)

```ts
function throwError(): never {
  throw new Error("Something went wrong");
}
```

---

# 🔹 8. unknown

👉 safe version of `any`

```ts
let value: unknown = "hello";
```

👉 use করার আগে type check করতে হবে

### ✅ Example

```ts
let value: unknown = "hello";

if (typeof value === "string") {
  console.log(value.toUpperCase());
}
```

### 🟢 Output:

```
HELLO
```

---

# 🔥 Type Inference (Auto Type Detection)

```ts
let name = "Fahim"; // string auto detect
let age = 25;      // number auto detect
```

👉 সবসময় manually type দিতে হয় না

---

# 🔴 Real Problem from your code

```ts
let userName = "mezba123";
let userId = 123;

userName.toFixed(); // ❌ error
userId.toFixed;     // ❌ call করা হয়নি
```

---

## ✅ Correct Version

```ts
let userName = "mezba123";
let userId = 123;

console.log(userId.toFixed(2)); // ✔
```

---

# 🧪 Full Working Example

```ts
"use strict";

let userName: string = "mezba123";
let userId: number = 123;

console.log(userName);
console.log(userId.toFixed(2));

let isAdmin: boolean = false;
isAdmin = true;
console.log(isAdmin);

let x: undefined = undefined;
console.log(x);

let y: null = null;
console.log(y);

function greet(): void {
  console.log("Hello World");
}
greet();

let value: unknown = "TypeScript";

if (typeof value === "string") {
  console.log(value.toUpperCase());
}
```

---

# 🟢 Final Output

```
mezba123
123.00
true
undefined
null
Hello World
TYPESCRIPT
```

---

# 🚀 Summary (Quick Revision)

* string → text
* number → সংখ্যা
* boolean → true/false
* undefined → value নাই
* null → empty value
* void → function return নাই
* never → function শেষ হয় না
* unknown → safe dynamic type

---

চাও হলে আমি এর পরের step হিসেবে **Non-Primitive Types (Array, Object, Tuple)** বা **TypeScript real project setup** একদম roadmap আকারে বুঝিয়ে দিতে পারি 👍

---
---
---

এই কোডটা আসলে TypeScript দিয়ে লেখা, কিন্তু পরে এটা JavaScript এ কম্পাইল (convert) হয়েছে। তাই কিছু জায়গায় ভুল/অদ্ভুত আচরণ দেখাবে।

চল ধাপে ধাপে সহজভাবে বুঝি 👇

---

## 🔹 Primitive Types কি?

Primitive types মানে হলো basic data types। যেমন:

* `string` → লেখা (text)
* `number` → সংখ্যা
* `boolean` → true/false
* `undefined` → value assign করা হয়নি
* `null` → intentionally empty

TypeScript এ আরও আছে:

* `never`
* `unknown`
* `void`

---

## 🔹 কোডটা দেখি

```ts
"use strict";
```

👉 এটা strict mode enable করে
মানে ভুল করলে JS বেশি strict ভাবে error ধরবে

---

```ts
let userName = "mezba123";
let userId = 123;
```

👉 এখানে:

* `userName` → string
* `userId` → number

TypeScript নিজেই type বুঝে নেয় (type inference)

---

## 🔴 Problem 1

```ts
userName.toFixed();
```

👉 এখানে error হবে ❌

কারণ:

* `toFixed()` শুধু number এর জন্য
* কিন্তু `userName` একটা string

### ✅ Output / Error:

```
TypeError: userName.toFixed is not a function
```

---

## 🔴 Problem 2

```ts
userId.toFixed;
```

👉 এখানে function call করা হয়নি ❌

এটা শুধু reference নিচ্ছে, run করছে না

### 👉 সঠিক হলে:

```ts
userId.toFixed();
```

### ✅ Output:

```
"123"
```

👉 `toFixed()` number কে string বানায় (decimal control সহ)

---

## 🔹 Boolean

```ts
let isAdmin = false;
isAdmin = true;
```

👉 এখানে:

* প্রথমে `false`
* পরে `true` assign করা হয়েছে

### ✅ Output:

```
true
```

---

## 🔹 Undefined

```ts
let x = undefined;
```

👉 মানে variable declare করা হয়েছে, কিন্তু কোনো meaningful value নাই

### ✅ Output:

```
undefined
```

---

## 🔹 Full Working Example (Correct Code)

```ts
"use strict";

let userName = "mezba123";
let userId = 123;

console.log(userName);       // mezba123
console.log(userId.toFixed(2)); // 123.00

let isAdmin = false;
isAdmin = true;
console.log(isAdmin);        // true

let x = undefined;
console.log(x);              // undefined
```

---

## 🔹 Output

```
mezba123
123.00
true
undefined
```

---

## 🔥 Key Learning

1. string এ number method ব্যবহার করা যাবে না
2. function call করতে `()` লাগবে
3. TypeScript অনেক error আগে থেকেই ধরতে সাহায্য করে
4. primitive type ঠিকভাবে বুঝা খুব important

---

চাও হলে আমি `never`, `unknown`, `void` এই advanced type গুলাও real example দিয়ে বুঝিয়ে দিতে পারি 👍

---
---
---

<img width="1309" height="473" alt="image" src="https://github.com/user-attachments/assets/2715a9bc-5e67-4fa9-a52c-97a9a581b020" />

