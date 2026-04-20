### 🔹 Normalizing using Lookup Table (সহজ ভাষায়)

ধরো তোমার কাছে এমন data আছে যেখানে একই তথ্য বারবার repeat হচ্ছে। এতে memory নষ্ট হয়, update করা কঠিন হয়, আর performance খারাপ হতে পারে।

এই সমস্যা সমাধানের জন্য আমরা **Normalization** করি।
আর JavaScript-এ খুব common একটা pattern হলো **Lookup Table ব্যবহার করা**।

---

## 🔸 Concept কী?

👉 **Lookup Table** মানে হলো এমন একটা object বা map, যেখানে key দিয়ে খুব দ্রুত data পাওয়া যায়।

👉 Array এর ভেতরে খুঁজতে গেলে `O(n)` লাগে
👉 Lookup table (object/map) হলে `O(1)` time লাগে

---

## 🔸 Example (Before Normalization)

```js
const users = [
  { id: 1, name: "Rahim" },
  { id: 2, name: "Karim" },
];

const posts = [
  { id: 101, title: "Post 1", user: { id: 1, name: "Rahim" } },
  { id: 102, title: "Post 2", user: { id: 2, name: "Karim" } },
];
```

### ❌ Problem:

* user data বারবার duplicate হচ্ছে
* update করলে সব জায়গায় change করতে হবে

---

## 🔸 After Normalization (Using Lookup Table)

```js
const users = {
  1: { id: 1, name: "Rahim" },
  2: { id: 2, name: "Karim" },
};

const posts = [
  { id: 101, title: "Post 1", userId: 1 },
  { id: 102, title: "Post 2", userId: 2 },
];
```

---

## 🔸 Data Access (Lookup)

```js
const post = posts[0];

const user = users[post.userId];

console.log(user.name);
```

### ✅ Output:

```
Rahim
```

---

## 🔸 Syntax Pattern (General)

```js
// Step 1: Normalize into lookup table
const lookup = data.reduce((acc, item) => {
  acc[item.id] = item;
  return acc;
}, {});

// Step 2: Use id reference
const result = lookup[someId];
```

---

## 🔸 Full Example with reduce()

```js
const usersArray = [
  { id: 1, name: "Rahim" },
  { id: 2, name: "Karim" },
];

// lookup table বানানো
const usersLookup = usersArray.reduce((acc, user) => {
  acc[user.id] = user;
  return acc;
}, {});

console.log(usersLookup);
```

### ✅ Output:

```js
{
  1: { id: 1, name: "Rahim" },
  2: { id: 2, name: "Karim" }
}
```

---

## 🔸 কেন ব্যবহার করবো?

✔ Fast access (O(1))
✔ Duplicate data কমে
✔ Update করা সহজ
✔ Clean structure (Redux, backend API-তে খুব common)

---

## 🔚 Short Summary

* Array → iterate করতে হয়
* Lookup table → direct access পাওয়া যায়
* Normalization → data কে efficient করে

---

তুমি চাইলে আমি এটাকে **Redux / real project example** দিয়ে আরও practicalভাবে দেখাতে পারি 👍

---
---
---
চল তোমার এই example টা step-by-step সহজভাবে বুঝি 👇

---

## 🔸 Problem কি?

তোমার কাছে একটা **array of objects (`postsArray`)** আছে।
তুমি এটাকে convert করতে চাও একটা **lookup table (object)**-এ, যেখানে:

👉 `id` হবে key
👉 পুরো object হবে value

---

## 🔸 Input

```js
const postsArray = [
  { id: "p-101", title: "Intro to SQL", author: "Alex" },
  { id: "p-102", title: "Data Structures in JS", author: "Beth" },
  { id: "p-103", title: "Understanding Reduce", author: "Chris" },
  { id: "p-104", title: "CSS Grid Tricks", author: "Alex" },
];
```

---

## 🔸 Solution (reduce দিয়ে Lookup Table তৈরি)

```js
const postsLookup = postsArray.reduce((acc, post) => {
  acc[post.id] = post;
  return acc;
}, {});

console.log(postsLookup);
```

---

## 🔸 Syntax Breakdown

```js
array.reduce((accumulator, currentItem) => {
  // logic
  return accumulator;
}, initialValue);
```

👉 এখানে:

* `acc` = accumulator (final object তৈরি হবে এখানে)
* `post` = প্রতিটা item (array থেকে এক এক করে আসবে)
* `{}` = শুরুতে empty object

---

## 🔸 Step-by-step কী হচ্ছে?

### 1st iteration:

```js
acc = {}
post = { id: "p-101", ... }

acc["p-101"] = post
```

👉 acc এখন:

```js
{
  "p-101": { id: "p-101", title: "Intro to SQL", author: "Alex" }
}
```

---

### 2nd iteration:

```js
acc["p-102"] = post
```

👉 acc:

```js
{
  "p-101": {...},
  "p-102": { id: "p-102", title: "Data Structures in JS", author: "Beth" }
}
```

---

👉 এইভাবে সবগুলো element add হবে

---

## 🔸 Final Output

```js
{
  "p-101": { id: "p-101", title: "Intro to SQL", author: "Alex" },
  "p-102": { id: "p-102", title: "Data Structures in JS", author: "Beth" },
  "p-103": { id: "p-103", title: "Understanding Reduce", author: "Chris" },
  "p-104": { id: "p-104", title: "CSS Grid Tricks", author: "Alex" }
}
```

---

## 🔸 কেন এটা useful?

✔ `postsLookup["p-103"]` দিয়ে direct access পাওয়া যায়
✔ loop করার দরকার নেই
✔ performance better (O(1) access)

---

## 🔚 Short Summary

* Array → iterate করতে হয়
* Lookup object → direct access
* `reduce()` → array কে object-এ convert করার best way

---

তুমি চাইলে আমি এটা **Map দিয়ে** বা **real project (API response)** দিয়ে দেখাতে পারি 👍

