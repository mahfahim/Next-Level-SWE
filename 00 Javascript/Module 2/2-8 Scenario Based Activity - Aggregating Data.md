এই টপিকটা আসলে **real-life data নিয়ে কাজ করার মতো একটা scenario**, যেখানে আমরা `reduce()` ব্যবহার করে data aggregate (সংগ্রহ/গণনা/summary তৈরি) করি।

চল একটা practical উদাহরণ দিয়ে বুঝি 👇

---

## 🔹 Scenario: Online Shop Cart Total বের করা

ধরি, একজন user কিছু পণ্য কিনেছে। এখন আমাদের কাজ:
👉 সব product এর total price (subtotal) বের করা

---

## 🔸 Input Data

```js
const cartItems = [
  { name: "Laptop", price: 50000, quantity: 1 },
  { name: "Mouse", price: 500, quantity: 2 },
  { name: "Keyboard", price: 1500, quantity: 1 },
];
```

---

## 🔸 Syntax (reduce)

```js
array.reduce((accumulator, currentValue) => {
  // logic
}, initialValue);
```

👉 এখানে:

* `accumulator` → আগের total (result জমা হয় এখানে)
* `currentValue` → current item
* `initialValue` → শুরুতে value (সাধারণত 0)

---

## 🔸 Code (Aggregation)

```js
const totalPrice = cartItems.reduce((total, item) => {
  return total + item.price * item.quantity;
}, 0);

console.log(totalPrice);
```

---

## 🔸 Step by Step Breakdown

| Step | item     | calculation    | total |
| ---- | -------- | -------------- | ----- |
| 1    | Laptop   | 50000 × 1      | 50000 |
| 2    | Mouse    | 500 × 2 = 1000 | 51000 |
| 3    | Keyboard | 1500 × 1       | 52500 |

---

## 🔸 Output

```js
52500
```

---

## 🔹 আরেকটা Scenario: Survey Count করা

ধরি users A, B, C option select করেছে। এখন count করতে হবে কে কয়বার এসেছে।

### Input

```js
const responses = ["A", "B", "A", "C", "B", "A"];
```

---

### Code

```js
const result = responses.reduce((acc, item) => {
  acc[item] = (acc[item] || 0) + 1;
  return acc;
}, {});

console.log(result);
```

---

### Output

```js
{
  A: 3,
  B: 2,
  C: 1
}
```

---

## 🔹 মূল ধারণা (Key Idea)

👉 Aggregating Data মানে:

* total বের করা (sum)
* count করা
* group করা
* summary বানানো

👉 `reduce()` দিয়ে আমরা:
➡️ একটা array → একটাই final result বানাই

---

## 🔹 কখন ব্যবহার করবে?

* Cart total
* ভোট/Survey count
* Salary total
* Grouping data
* Analytics / Report

---

যদি চাও, আমি তোমাকে **আরও advanced scenario (group by + sum একসাথে)** দেখাতে পারি, যেটা real project এ খুব বেশি লাগে 👍


---
---
---

এখানে কাজটা দুইটা একসাথে করতে হবে 👉

1. **Grouping** (category অনুযায়ী ভাগ করা)
2. **Aggregating** (total revenue + item count বের করা)

এই কাজটা সবচেয়ে সহজভাবে করা যায় `reduce()` দিয়ে।

---

## 🔹 Input Data

```js
const sales = [
  { category: "Electronics", item: "Laptop", price: 1200, quantity: 1 },
  { category: "Books", item: "JS Basics", price: 30, quantity: 2 },
  { category: "Electronics", item: "Mouse", price: 25, quantity: 2 },
  { category: "Home", item: "Chair", price: 150, quantity: 1 },
  { category: "Books", item: "React Deep Dive", price: 50, quantity: 1 },
  { category: "Electronics", item: "Keyboard", price: 80, quantity: 1 },
];
```

---

## 🔹 Syntax (reduce)

```js
array.reduce((acc, item) => {
  // logic
  return acc;
}, initialValue);
```

👉 `acc` = final result object
👉 `item` = array এর প্রতিটি object

---

## 🔹 Code (Grouping + Aggregation)

```js
const result = sales.reduce((acc, item) => {
  const { category, price, quantity } = item;

  // যদি category আগে না থাকে, initialize করো
  if (!acc[category]) {
    acc[category] = {
      totalRevenue: 0,
      itemCount: 0,
    };
  }

  // revenue যোগ করা
  acc[category].totalRevenue += price * quantity;

  // item count যোগ করা
  acc[category].itemCount += quantity;

  return acc;
}, {});

console.log(result);
```

---

## 🔹 Step by Step বুঝি

### 👉 1st item:

```js
{ category: "Electronics", price: 1200, quantity: 1 }
```

```js
Electronics = {
  totalRevenue: 1200,
  itemCount: 1
}
```

---

### 👉 2nd item:

```js
{ category: "Books", price: 30, quantity: 2 }
```

```js
Books = {
  totalRevenue: 60,
  itemCount: 2
}
```

---

### 👉 3rd item:

```js
{ category: "Electronics", price: 25, quantity: 2 }
```

আগের Electronics এর সাথে যোগ হবে:

```js
Electronics = {
  totalRevenue: 1200 + 50 = 1250,
  itemCount: 1 + 2 = 3
}
```

---

(এইভাবে সব item iterate হবে)

---

## 🔹 Final Output

```js
{
  Electronics: {
    totalRevenue: 1330,
    itemCount: 4,
  },
  Books: {
    totalRevenue: 110,
    itemCount: 3,
  },
  Home: {
    totalRevenue: 150,
    itemCount: 1,
  },
}
```

---

## 🔹 মূল ধারণা (Important Insight)

👉 `reduce()` দিয়ে আমরা:

* dynamic key (category) তৈরি করছি
* একই key হলে update করছি
* শেষে একটা grouped summary পাচ্ছি

👉 এই pattern টাকে বলে:
**“Group By + Aggregate Pattern”**

---

## 🔹 শর্টভাবে বুঝলে

👉 `if (!acc[category])` → নতুন group তৈরি
👉 `+=` → data accumulate (যোগ করা)

---

চাও হলে আমি এটাকে আরও simplify করে **visual diagram বা animation style explanation** দিয়েও দেখাতে পারি 👍
