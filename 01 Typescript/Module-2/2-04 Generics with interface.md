চলো একদম সহজ টিউটোরিয়াল স্টাইলে **Generic with Interface** বুঝি, সাথে output-ও দেখবো।

---

# 🧠 Generic with Interface (সহজ টিউটোরিয়াল)

## 🔥 1. Generic আসলে কী?

Generic মানে:
👉 “একই structure, কিন্তু বিভিন্ন type দিয়ে ব্যবহার করা যায়”

মানে তুমি একবার কোড লিখবে, পরে সেটা string, number, object যেকোনো কিছুতে ব্যবহার করতে পারবে।

---

# 🔷 2. Interface ছাড়া সমস্যা

ধরি তুমি শুধু number দিয়ে একটা list বানাতে চাও:

```ts
const numbers: number[] = [1, 2, 3];
```

এটা ঠিক আছে।

কিন্তু যদি string লাগে?

```ts
const names: string[] = ["A", "B", "C"];
```

👉 দুইটা আলাদা আলাদা লিখতে হচ্ছে।

---

# 🔷 3. Generic Interface (Main Concept)

এখন আমরা এটা reusable বানাবো:

```ts
interface GenericArray<T> {
  [index: number]: T;
}
```

---

## 🧠 এখানে কী হচ্ছে?

* `T` = যেকোনো type (placeholder)
* `T` পরে replace হবে string / number / boolean ইত্যাদি দিয়ে

---

# 🔷 4. Example 1: Number Array

```ts
const numbers: GenericArray<number> = [10, 20, 30];
```

👉 এখানে `T = number`

---

## ✔ Output:

```ts
[10, 20, 30]
```

---

# 🔷 5. Example 2: String Array

```ts
const friends: GenericArray<string> = ["Rahim", "Karim", "Jabbar"];
```

👉 এখানে `T = string`

---

## ✔ Output:

```ts
["Rahim", "Karim", "Jabbar"]
```

---

# 🔷 6. Example 3: Boolean Array

```ts
const flags: GenericArray<boolean> = [true, false, true];
```

👉 এখানে `T = boolean`

---

## ✔ Output:

```ts
[true, false, true]
```

---

# 🔥 7. Real meaning

Generic Interface মানে:

👉 “একটা template বানানো, যেটা যেকোনো type নিতে পারে”

---

# 🧾 সহজভাবে মনে রাখো

| Concept           | Meaning                 |
| ----------------- | ----------------------- |
| `T`               | placeholder type        |
| `GenericArray<T>` | reusable type structure |
| `<number>`        | number version          |
| `<string>`        | string version          |

---

# 🚀 Final Summary

Generic Interface তোমাকে দেয়:

✔ Reusability
✔ Type safety
✔ Less code duplication
✔ Flexible design

---

চাও হলে আমি পরের ধাপে **Generic Function (real interview level)** একদম সহজভাবে বুঝিয়ে দিতে পারি।

