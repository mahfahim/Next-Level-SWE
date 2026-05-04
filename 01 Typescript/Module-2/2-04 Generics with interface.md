চলো এখন **Generic with Interface** একদম ডিটেইল কিন্তু সহজভাবে বুঝি, টিউটোরিয়াল স্টাইলে। তুমি ধাপে ধাপে ধরলে এটা অনেক ক্লিয়ার হয়ে যাবে।

---

# 🧠 Generic with Interface (Full Detailed Tutorial)

## 🔥 1. Generic আসলে কী সমস্যা solve করে?

ধরি তুমি ৩টা আলাদা array বানাতে চাও:

```ts
const numbers = [1, 2, 3];
const names = ["A", "B", "C"];
const booleans = [true, false];
```

এখানে সমস্যা হলো:

👉 একই pattern বারবার লিখতে হচ্ছে
👉 type আলাদা হলে নতুন কোড লিখতে হচ্ছে

---

# 🔷 2. Interface দিয়ে শুরু করি (Generic ছাড়া)

ধরি আমরা number array define করলাম:

```ts id="o0r7kc"
interface NumberArray {
  [index: number]: number;
}
```

ব্যবহার:

```ts id="7v6x7c"
const nums: NumberArray = [1, 2, 3];
```

✔ ঠিক আছে, কিন্তু সমস্যা:

👉 এখন যদি string লাগে? আবার নতুন interface লিখতে হবে

---

# 😵 Problem:

```ts
interface StringArray {
  [index: number]: string;
}
```

👉 code duplicate হচ্ছে

---

# 🔥 3. Generic Interface (Solution)

এখানেই Generic আসে:

```ts id="4q1q6q"
interface GenericArray<T> {
  [index: number]: T;
}
```

---

## 🧠 এখানে কী হচ্ছে?

| অংশ                  | মানে                                |
| -------------------- | ----------------------------------- |
| `T`                  | একটা placeholder type               |
| `GenericArray<T>`    | যেকোনো type নিতে পারবে              |
| `[index: number]: T` | array-এর প্রতিটা element হবে T type |

---

# 🔷 4. এখন ব্যবহার দেখি

## 👉 Number version

```ts id="9l9z0h"
const numbers: GenericArray<number> = [10, 20, 30];
```

### এখানে:

* `T = number`

---

## ✔ Output:

```ts
[10, 20, 30]
```

---

# 🔷 5. String version

```ts id="k1x2qv"
const friends: GenericArray<string> = ["Rahim", "Karim", "Jabbar"];
```

### এখানে:

* `T = string`

---

## ✔ Output:

```ts
["Rahim", "Karim", "Jabbar"]
```

---

# 🔷 6. Boolean version

```ts id="z8m1a2"
const flags: GenericArray<boolean> = [true, false, true];
```

### এখানে:

* `T = boolean`

---

## ✔ Output:

```ts
[true, false, true]
```

---

# 🔥 7. আসলে ভিতরে কী হচ্ছে?

Generic কাজ করে এমনভাবে:

### তুমি লিখলে:

```ts
GenericArray<string>
```

### TypeScript internally বুঝে নেয়:

```ts
interface GenericArray {
  [index: number]: string;
}
```

---

# 🧠 8. Real Life Example (খুব important)

ধরি API থেকে data আসছে:

```ts
interface ApiResponse<T> {
  status: number;
  data: T;
}
```

---

## 👉 User API

```ts
const user: ApiResponse<{ name: string }> = {
  status: 200,
  data: {
    name: "Fahim",
  },
};
```

---

## 👉 Product API

```ts
const product: ApiResponse<{ price: number }> = {
  status: 200,
  data: {
    price: 500,
  },
};
```

---

# 🚀 9. Generic কেন powerful?

✔ Same interface reuse করা যায়
✔ Type safety থাকে
✔ Bug কম হয়
✔ Code clean হয়
✔ Interview favourite topic

---

# 🧾 10. Final Easy Summary

👉 Generic Interface মানে:

> “একটা template বানানো, যেটা পরে যেকোনো type দিয়ে fill করা যায়”

---

## 🔥 মনে রাখার trick:

* `T` = Type placeholder
* `<string>` = string version
* `<number>` = number version
* Interface = structure
* Generic = flexible structure

---

# 💡 One-line concept

👉 Generic Interface = Reusable type blueprint

---

চাও হলে আমি পরের ধাপে **Generic Function + Real Interview Questions** একদম সহজ করে শিখিয়ে দিতে পারি, যেটা interview এ অনেক আসে।
