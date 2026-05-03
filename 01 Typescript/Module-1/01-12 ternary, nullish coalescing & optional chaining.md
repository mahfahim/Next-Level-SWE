নিচে তোমার জন্য **“01–12 Ternary, Nullish Coalescing & Optional Chaining in TypeScript” ফুল টিউটোরিয়াল** একদম সহজ বাংলায়, উদাহরণ + console output সহ সাজানো হলো।

---

# 📘 TypeScript Tutorial: 01–12

## Ternary, Nullish Coalescing & Optional Chaining

এই তিনটা concept real project-এ খুব বেশি ব্যবহার হয়:

* `? :` → decision নেওয়ার জন্য
* `??` → default value দেওয়ার জন্য
* `?.` → safe access করার জন্য

---

# 🔹 01. Ternary Operator কী?

👉 এটা `if-else` এর shortcut

```ts id="t1"
const age = 22;

const result = age >= 21 ? "Eligible" : "Not Eligible";

console.log(result);
```

### 🖥️ Output:

```txt id="ot1"
Eligible
```

---

# 🔹 02. Ternary vs if-else

```ts id="t2"
// if-else
let message;

if (age >= 21) {
  message = "Eligible";
} else {
  message = "Not Eligible";
}

// ternary
const msg = age >= 21 ? "Eligible" : "Not Eligible";

console.log(msg);
```

### 🖥️ Output:

```txt id="ot2"
Eligible
```

---

# 🔹 03. Function এ Ternary

```ts id="t3"
const biyerJonnoEligible = (age: number) => {
  return age >= 21 ? "You are eligible" : "You are not eligible!";
};

console.log(biyerJonnoEligible(18));
```

### 🖥️ Output:

```txt id="ot3"
You are not eligible!
```

---

# 🔹 04. Nested Ternary (careful use)

```ts id="t4"
const marks = 75;

const grade =
  marks >= 80 ? "A+" :
  marks >= 70 ? "A" :
  marks >= 60 ? "B" :
  "F";

console.log(grade);
```

### 🖥️ Output:

```txt id="ot4"
A
```

---

# 🔹 05. Nullish Coalescing (`??`) কী?

👉 শুধু `null` বা `undefined` হলে fallback value দেয়

```ts id="n1"
const userTheme = null;

const theme = userTheme ?? "Light Theme";

console.log(theme);
```

### 🖥️ Output:

```txt id="on1"
Light Theme
```

---

# 🔹 06. Nullish vs OR (`||`) difference

```ts id="n2"
const value = "";

// OR operator
const result1 = value || "Default";

// Nullish operator
const result2 = value ?? "Default";

console.log(result1);
console.log(result2);
```

### 🖥️ Output:

```txt id="on2"
Default
""
```

## 🧠 ব্যাখ্যা:

* `""` → falsy → `||` এটাকে reject করে
* কিন্তু `??` এটাকে accept করে (কারণ null/undefined না)

---

# 🔹 07. Real Example (Auth System)

```ts id="n3"
const isAuthenticated = "";

const user = isAuthenticated ?? "Guest";

console.log(user);
```

### 🖥️ Output:

```txt id="on3"
""
```

---

# 🔹 08. Optional Chaining (`?.`) কী?

👉 object এর property safely access করতে

```ts id="o1"
const user = {
  address: {
    city: "Dhaka",
  },
};

const city = user?.address?.city;

console.log(city);
```

### 🖥️ Output:

```txt id="oo1"
Dhaka
```

---

# 🔹 09. Optional Chaining without crash

```ts id="o2"
const user = {};

const city = user?.address?.city;

console.log(city);
```

### 🖥️ Output:

```txt id="oo2"
undefined
```

👉 error দিবে না, safe ভাবে undefined দিবে

---

# 🔹 10. Optional Chaining with Function

```ts id="o3"
const user: any = {
  greet: () => "Hello!",
};

console.log(user?.greet?.());
```

### 🖥️ Output:

```txt id="oo3"
Hello!
```

---

# 🔹 11. Combine Example (All together)

```ts id="c1"
const user = {
  name: "Fahim",
  address: null,
};

const city = user?.address?.city ?? "No City Found";

console.log(city);
```

### 🖥️ Output:

```txt id="oc1"
No City Found
```

---

# 🔹 12. Real World Example

```ts id="c2"
type User = {
  name: string;
  theme?: string | null;
};

const user: User = {
  name: "Fahim",
  theme: null,
};

const theme = user.theme ?? "Default Theme";

const message = user.name ? user.name : "Guest";

console.log(theme);
console.log(message);
```

### 🖥️ Output:

```txt id="oc2"
Default Theme
Fahim
```

---

# 🔥 Final Summary

| Operator | কাজ                         |
| -------- | --------------------------- |
| `? :`    | decision (if-else shortcut) |
| `??`     | null/undefined হলে default  |
| `?.`     | safe access                 |

---

# 🧠 মনে রাখার shortcut

* Ternary → **decision**
* Nullish → **default value**
* Optional chaining → **safe access**

---

# 🚀 Next Step

এখন এগুলো শিখলে তুমি আরও strong হবে:

* Type narrowing
* Advanced union handling
* Error handling with optional chaining
* Real API response handling

---

চাওলে আমি next part বানিয়ে দিতে পারি:
👉 **“13–20 Advanced TypeScript + Real Project (Auth + API + Error Handling)”**
