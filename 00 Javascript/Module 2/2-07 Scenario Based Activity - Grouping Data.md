“Scenario Based Activity - Grouping Data” মানে হলো কোনো real-life data কে condition অনুযায়ী আলাদা আলাদা group-এ ভাগ করা। JavaScript-এ সাধারণত এটা করা হয় `reduce()` ব্যবহার করে।

চলো সহজভাবে বুঝি 👇

---

## 🔹 Problem Scenario

ধরো তোমার কাছে কিছু products আছে, এবং তুমি এগুলোকে category অনুযায়ী group করতে চাও।

```js
const products = [
  { id: 1, name: "Laptop", category: "Electronics" },
  { id: 2, name: "Shirt", category: "Clothing" },
  { id: 3, name: "Mobile", category: "Electronics" },
  { id: 4, name: "Jeans", category: "Clothing" },
  { id: 5, name: "Headphone", category: "Electronics" },
];
```

---

## 🔹 Goal

Output এমন হবে:

```js
{
  Electronics: [
    { id: 1, name: "Laptop", category: "Electronics" },
    { id: 3, name: "Mobile", category: "Electronics" },
    { id: 5, name: "Headphone", category: "Electronics" }
  ],
  Clothing: [
    { id: 2, name: "Shirt", category: "Clothing" },
    { id: 4, name: "Jeans", category: "Clothing" }
  ]
}
```

---

## 🔹 Syntax (reduce)

```js
array.reduce((accumulator, currentItem) => {
  // logic
  return accumulator;
}, initialValue);
```

* `accumulator` → যেখানে result জমা হবে
* `currentItem` → current array element
* `initialValue` → শুরুতে empty object `{}`

---

## 🔹 Full Code (Grouping)

```js
const groupedProducts = products.reduce((acc, product) => {
  const key = product.category;

  // যদি category আগে না থাকে
  if (!acc[key]) {
    acc[key] = [];
  }

  // সেই category তে item push করি
  acc[key].push(product);

  return acc;
}, {});

console.log(groupedProducts);
```

---

## 🔹 Step-by-step Explanation

👉 1. প্রথমে `acc = {}` (empty object)

👉 2. প্রথম item:

```js
{ id: 1, name: "Laptop", category: "Electronics" }
```

* acc["Electronics"] নেই → create করলাম `[]`
* push করলাম Laptop

👉 acc এখন:

```js
{
  Electronics: [Laptop]
}
```

---

👉 3. দ্বিতীয় item (Shirt - Clothing)

```js
{
  Electronics: [...],
  Clothing: [Shirt]
}
```

---

👉 এভাবে loop শেষে final object তৈরি হয়

---

## 🔹 Final Output

```js
{
  Electronics: [
    { id: 1, name: "Laptop", category: "Electronics" },
    { id: 3, name: "Mobile", category: "Electronics" },
    { id: 5, name: "Headphone", category: "Electronics" }
  ],
  Clothing: [
    { id: 2, name: "Shirt", category: "Clothing" },
    { id: 4, name: "Jeans", category: "Clothing" }
  ]
}
```

---

## 🔹 Short Trick Version (Advanced)

```js
const grouped = products.reduce((acc, p) => {
  (acc[p.category] = acc[p.category] || []).push(p);
  return acc;
}, {});
```

---

## 🔹 Real-life Use Cases

* category wise product list
* student কে grade অনুযায়ী ভাগ করা
* user কে role অনুযায়ী group করা
* order কে status অনুযায়ী ভাগ করা

---

তুমি চাইলে আমি এটা “student marks grouping” বা “order status grouping” দিয়েও explain করতে পারি 👍

---
---
---

এটা একটা classic **Grouping + Aggregation** problem। এখানে আমরা দুইটা কাজ একসাথে করছি:

* **Grouping** → কোন response (A, B, C, D) কতবার এসেছে
* **Aggregation** → প্রতিটার count বের করা

---

## 🔹 Input

```js
const surveyResponses = [
  "A", "C", "B", "A", "B", "B",
  "C", "A", "B", "D", "A", "C",
  "B", "A",
];
```

---

## 🔹 Goal Output

```js
{ A: 5, C: 3, B: 5, D: 1 }
```

মানে:

* A → 5 বার
* B → 5 বার
* C → 3 বার
* D → 1 বার

---

## 🔹 Syntax (reduce)

```js
array.reduce((acc, current) => {
  // logic
  return acc;
}, initialValue);
```

* `acc` → result object (count জমা রাখবে)
* `current` → current response (A, B, C...)
* `initialValue` → `{}`

---

## 🔹 Full Code

```js
const result = surveyResponses.reduce((acc, response) => {
  // যদি key আগে না থাকে, তাহলে 0 দিয়ে শুরু
  if (!acc[response]) {
    acc[response] = 0;
  }

  // count বাড়াই
  acc[response]++;

  return acc;
}, {});

console.log(result);
```

---

## 🔹 Step-by-step Explanation

👉 শুরুতে:

```js
acc = {}
```

---

👉 1st item: `"A"`

```js
acc["A"] = 0 → তারপর +1
```

👉 এখন:

```js
{ A: 1 }
```

---

👉 2nd item: `"C"`

```js
{ A: 1, C: 1 }
```

---

👉 3rd item: `"B"`

```js
{ A: 1, C: 1, B: 1 }
```

---

👉 আবার `"A"` আসলে:

```js
{ A: 2, C: 1, B: 1 }
```

---

👉 এভাবে loop চলতে থাকবে...

---

## 🔹 Final Output

```js
{ A: 5, C: 3, B: 5, D: 1 }
```

---

## 🔹 Short Trick Version

```js
const result = surveyResponses.reduce((acc, r) => {
  acc[r] = (acc[r] || 0) + 1;
  return acc;
}, {});
```

👉 এখানে:

* `acc[r] || 0` → যদি না থাকে, 0 ধরে
* তারপর +1

---

## 🔹 Real-life Use Cases

* survey result count
* ভোট গণনা
* product rating count
* log analysis (error count)

---

চাও হলে আমি এটা step-by-step table আকারে (iteration wise) দেখাতে পারি, তাহলে আরও clear হবে 👍
