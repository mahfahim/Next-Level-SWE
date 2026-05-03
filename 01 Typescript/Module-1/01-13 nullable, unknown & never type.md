নিচে তোমার জন্য **“01–13 Nullable, Unknown & Never Type in TypeScript” ফুল টিউটোরিয়াল** একদম সহজ বাংলায়, উদাহরণ + console output সহ সাজানো হলো।

---

# 📘 TypeScript Tutorial: 01–13

## Nullable, Unknown & Never Type

এই তিনটা concept real project-এ খুব দরকারি:

* **Nullable** → value থাকতে পারে বা নাও থাকতে পারে
* **unknown** → type জানা নেই, আগে check করতে হবে
* **never** → function কখনো return করে না

---

# 🔹 01. Nullable Type কী?

👉 যখন কোনো variable `null` হতে পারে

```ts id="n1"
let userName: string | null;

userName = "Fahim";
console.log(userName);

userName = null;
console.log(userName);
```

### 🖥️ Output:

```txt id="on1"
Fahim
null
```

---

# 🔹 02. Function with Nullable

```ts id="n2"
const getUser = (input: string | null) => {
  if (input) {
    console.log(`From DB: ${input}`);
  } else {
    console.log("From DB: ALL USER");
  }
};

getUser("Rahim");
getUser(null);
```

### 🖥️ Output:

```txt id="on2"
From DB: Rahim
From DB: ALL USER
```

---

# 🔹 03. Null Check Problem

```ts id="n3"
const printLength = (text: string | null) => {
  // console.log(text.length); ❌ error হতে পারে

  if (text !== null) {
    console.log(text.length);
  }
};

printLength("Hello");
```

### 🖥️ Output:

```txt id="on3"
5
```

👉 null check না করলে crash হতে পারে

---

# 🔹 04. Unknown Type কী?

👉 unknown মানে → type জানা নেই

```ts id="u1"
let data: unknown;

data = 100;
data = "Hello";
data = true;
```

👉 কিন্তু সরাসরি use করা যাবে না

---

# 🔹 05. Unknown with Type Check

```ts id="u2"
const process = (input: unknown) => {
  if (typeof input === "string") {
    console.log(input.toUpperCase());
  } else if (typeof input === "number") {
    console.log(input * 2);
  } else {
    console.log("Unknown type");
  }
};

process("hello");
process(10);
process(true);
```

### 🖥️ Output:

```txt id="ou2"
HELLO
20
Unknown type
```

---

# 🔹 06. Real Example (Discount Calculator)

```ts id="u3"
const discountCalculator = (input: unknown) => {
  if (typeof input === "number") {
    console.log(input * 0.1);
  } else if (typeof input === "string") {
    const [price] = input.split(" ");
    console.log(Number(price) * 0.1);
  } else {
    console.log("Wrong input");
  }
};

discountCalculator(100);
discountCalculator("200 TK");
discountCalculator(null);
```

### 🖥️ Output:

```txt id="ou3"
10
20
Wrong input
```

---

# 🔹 07. unknown vs any

```ts id="u4"
let a: any = "Hello";
a.toUpperCase(); // ❌ check ছাড়া allowed

let b: unknown = "Hello";
// b.toUpperCase(); ❌ error
```

👉 unknown safer than any

---

# 🔹 08. Never Type কী?

👉 never মানে → function কখনো return করবে না

```ts id="nv1"
const throwError = (msg: string): never => {
  throw new Error(msg);
};

throwError("Something went wrong");
```

---

## 🖥️ Output:

```txt id="onv1"
Error: Something went wrong
```

👉 program এখানেই stop

---

# 🔹 09. Infinite Loop Example

```ts id="nv2"
const infiniteLoop = (): never => {
  while (true) {
    console.log("Running...");
  }
};
```

👉 কখনো শেষ হবে না

---

# 🔹 10. void vs never

```ts id="nv3"
// void
const sayHello = (): void => {
  console.log("Hello");
};

// never
const crash = (): never => {
  throw new Error("Crash!");
};
```

---

## 🧠 Difference

| Type  | মানে               |
| ----- | ------------------ |
| void  | কিছু return করে না |
| never | কখনো return করে না |

---

# 🔹 11. Real World Example

```ts id="rw1"
const login = (user: string | null) => {
  if (!user) {
    throw new Error("User not found");
  }

  console.log(`Welcome ${user}`);
};

login("Fahim");
// login(null); ❌ error throw করবে
```

### 🖥️ Output:

```txt id="orw1"
Welcome Fahim
```

---

# 🔥 Final Summary

## ✅ Nullable

👉 value থাকতে পারে বা null হতে পারে

## ✅ Unknown

👉 type unknown → আগে check করতে হবে

## ✅ Never

👉 function কখনো return করে না

---

# 🧠 সহজভাবে মনে রাখো

* Nullable → "থাকতেও পারে, না-ও থাকতে পারে"
* Unknown → "কি type জানি না"
* Never → "এখানেই শেষ"

---

# 🚀 Next Step

এখন এগুলো শিখলে তুমি আরও strong হবে:

* Type Narrowing
* Type Guards
* Advanced error handling
* API response typing

---

চাওলে আমি next part বানিয়ে দিতে পারি:
👉 **“14–20 Advanced TypeScript (Type Narrowing + Guards + Real Project)”**

