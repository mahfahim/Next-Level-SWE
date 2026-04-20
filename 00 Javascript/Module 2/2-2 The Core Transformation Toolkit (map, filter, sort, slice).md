চলো এখন `map`, `filter`, `sort`, `slice`—এই চারটা খুব গুরুত্বপূর্ণ array method **syntax সহ পরিষ্কারভাবে** দেখি।

---

## 1. `map()` Syntax

```js
array.map((element, index, array) => {
  // return transformed value
});
```

### Example:

```js
const numbers = [1, 2, 3];

const result = numbers.map((num) => {
  return num * 2;
});

console.log(result);
// [2, 4, 6]
```

👉 এখানে প্রতিটা element transform হচ্ছে

---

## 2. `filter()` Syntax

```js
array.filter((element, index, array) => {
  // return true or false
});
```

### Example:

```js
const numbers = [1, 2, 3, 4, 5];

const result = numbers.filter((num) => {
  return num % 2 === 0;
});

console.log(result);
// [2, 4]
```

👉 যেটা `true` হবে, শুধু সেটাই থাকবে

---

## 3. `sort()` Syntax

```js
array.sort((a, b) => {
  return a - b; // ascending
});
```

### Example:

```js
const numbers = [5, 1, 9, 3];

numbers.sort((a, b) => {
  return a - b;
});

console.log(numbers);
// [1, 3, 5, 9]
```

### Descending:

```js
numbers.sort((a, b) => b - a);
```

👉 important: এটা original array change করে

---

## 4. `slice()` Syntax

```js
array.slice(startIndex, endIndex);
```

👉 endIndex include হয় না

### Example:

```js
const numbers = [10, 20, 30, 40, 50];

const result = numbers.slice(1, 4);

console.log(result);
// [20, 30, 40]
```

👉 index 1 থেকে 3 পর্যন্ত নেয় (4 include না)

---

## Quick Cheat Sheet

```js
map()    → transform করে নতুন array
filter() → condition অনুযায়ী বাছাই
sort()   → array সাজায় (original change করে)
slice()  → অংশ কেটে নেয় (original change করে না)
```

---

চাও হলে আমি এগুলোর **real project example (API data / React / backend use)** দেখাতে পারি, তখন আরও clear হবে 👍

---
---
---
---


```
//? Input
const rawApiData = [
  {
    id: "p-001",
    productName: "Quantum Laptop", ///
    category: "Electronics",
    price: 1200,
    rating: 4.8,
    stock: 15,
  },
  {
    id: "p-002",
    productName: "The Art of Code",
    category: "Books",
    price: 45,
    rating: 4.5,
    stock: 100,
  },
  {
    id: "p-003",
    productName: "Cyber Hoodie",
    category: "Clothing",
    price: 80,
    rating: 4.7,
    stock: 50,
  },
  {
    id: "p-004",
    productName: "4K Drone",
    category: "Electronics",
    price: 650,
    rating: 4.3,
    stock: 20,
  },
  {
    id: "p-005",
    productName: "Basic JavaScript",
    category: "Books",
    price: 25,
    rating: 3.8,
    stock: 200,
  },
  {
    id: "p-006",
    productName: "Smart Watch",
    category: "Electronics",
    price: 250,
    rating: 4.7,
    stock: 70,
  },
  {
    id: "p-007",
    productName: "Classic T-Shirt",
    category: "Clothing",
    price: 30,
    rating: 4.2,
    stock: 300,
  },
  {
    id: "p-008",
    productName: "Design Patterns",
    category: "Books",
    price: 55,
    rating: 4.9,
    stock: 80,
  },
  {
    id: "p-009",
    productName: "VR Headset",
    category: "Electronics",
    price: 400,
    rating: 4.6,
    stock: 30,
  },
  {
    id: "p-010",
    productName: "USB-C Cable",
    category: "Electronics",
    price: 15,
    rating: 4.0,
    stock: 500,
  },
  {
    id: "p-011",
    productName: "Noise-Cancelling Headphones",
    category: "Electronics",
    price: 300,
    rating: 4.7,
    stock: 40,
  },
  {
    id: "p-012",
    productName: "Algorithms Explained",
    category: "Books",
    price: 50,
    rating: 4.5,
    stock: 60,
  },
];

//? Output => [{ name: "Phone" }, { name: "Smart Watch" }]

//* Process
//TODO Filter => Electronics
//TODO Sort by => Rating
//TODO Map => transform object shape to { name : "Name"}

const topElectronicProducts = rawApiData
  .filter((item) => item.category === "Electronics")
  .sort((a, b) => b.rating - a.rating)
  .slice(0, 3)
  .map((item) => {
    return { name: item.productName };
  });

console.log(topElectronicProducts);

```
