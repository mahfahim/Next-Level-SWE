JavaScript-এর `reduce()` হলো একটা খুব powerful method, যেটা দিয়ে তুমি একটা array থেকে **একটা single value** বানাতে পারো (যেমন: sum, average, object, ইত্যাদি)।

---

## 🔹 reduce() কী?

`reduce()` array-এর প্রতিটা element ঘুরে (iterate করে) একটা **final result তৈরি করে**।

---

## 🔹 Syntax

```js
array.reduce((accumulator, currentValue) => {
  return updatedAccumulator;
}, initialValue);
```

### বুঝে নিই:

* `accumulator` → আগের result (জমা হওয়া মান)
* `currentValue` → current element
* `initialValue` → শুরুতে accumulator-এর value

---

## 🔹 Example 1: Sum (Simple Total)

```js
const numbers = [1, 2, 3, 4];

const sum = numbers.reduce((acc, curr) => {
  return acc + curr;
}, 0);

console.log(sum);
```

### Output:

```
10
```

### Step-by-step:

| Step | acc | curr | result |
| ---- | --- | ---- | ------ |
| 1    | 0   | 1    | 1      |
| 2    | 1   | 2    | 3      |
| 3    | 3   | 3    | 6      |
| 4    | 6   | 4    | 10     |

---

## 🔹 Example 2: Max Number বের করা

```js
const numbers = [5, 2, 9, 1];

const max = numbers.reduce((acc, curr) => {
  return acc > curr ? acc : curr;
}, numbers[0]);

console.log(max);
```

### Output:

```
9
```

---

## 🔹 Example 3: Object বানানো (Aggregation)

```js
const fruits = ["apple", "banana", "apple", "orange", "banana"];

const count = fruits.reduce((acc, fruit) => {
  acc[fruit] = (acc[fruit] || 0) + 1;
  return acc;
}, {});

console.log(count);
```

### Output:

```js
{
  apple: 2,
  banana: 2,
  orange: 1
}
```

👉 এখানে `reduce()` দিয়ে আমরা array থেকে **grouping/counting object** বানিয়েছি।

---

## 🔹 Example 4: Average বের করা

```js
const numbers = [10, 20, 30];

const avg = numbers.reduce((acc, curr, index, arr) => {
  acc += curr;
  if (index === arr.length - 1) {
    return acc / arr.length;
  }
  return acc;
}, 0);

console.log(avg);
```

### Output:

```
20
```

---

## 🔹 reduce() কেন important?

* sum, average, max/min → সব করতে পারো
* array → object/number/string বানাতে পারো
* data transformation + aggregation একসাথে করতে পারো

---

## 🔹 Shortcut (Arrow Function)

```js
const sum = [1, 2, 3].reduce((acc, curr) => acc + curr, 0);
```

---

## 🔚 Summary

* `reduce()` = array → single value
* accumulator = জমা হওয়া result
* খুব flexible: sum, object, grouping, transformation সব করতে পারে

---

চাও হলে আমি `map + filter + reduce` একসাথে real-life example দিয়ে দেখাতে পারি 👍


---
---
---

চলো দুইটা example একদম সহজভাবে বুঝি — `reduce()` এখানে কীভাবে কাজ করছে, step-by-step 👇

---

# 🔹 1. Cart Subtotal (Total Price বের করা)

## ✅ Code

```js
const cartItems = [
  { id: "p-001", name: "Daraz Laptop Bag", price: 1500, quantity: 1 },
  { id: "p-002", name: "Walton USB-C Cable", price: 350, quantity: 2 },
  { id: "p-003", name: "Aarong Kurta", price: 2200, quantity: 1 },
];

const subtotal = cartItems.reduce((subtotal, product) => {
  return subtotal + product.price * product.quantity;
}, 0);

console.log(subtotal);
```

---

## 🔹 Syntax বোঝা

```js
array.reduce((accumulator, currentValue) => {}, initialValue)
```

এখানে:

* `subtotal` → accumulator (আগের total)
* `product` → current item
* `0` → initial value

---

## 🔹 Logic

👉 প্রতিটা product এর জন্য:

```
price × quantity = item total
```

তারপর সবগুলো যোগ হচ্ছে

---

## 🔹 Step-by-step Table

| Step | subtotal (acc) | product | calculation | result |
| ---- | -------------- | ------- | ----------- | ------ |
| 1    | 0              | 1500×1  | 1500        | 1500   |
| 2    | 1500           | 350×2   | 700         | 2200   |
| 3    | 2200           | 2200×1  | 2200        | 4400   |

---

## ✅ Output

```js
4400
```

---

# 🔹 2. Best Scorer খুঁজে বের করা

## ✅ Code

```js
const players = [
  { name: "Jamal Bhuyan", score: 88 },
  { name: "Shekh Morsalin", score: 81 },
  { name: "Rakib Hossain", score: 95 },
  { name: "Topu Barman", score: 91 },
  { name: "Sohel Rana", score: 72 },
];

const bestScorer = players.reduce((bestPlayer, player) => {
  if (bestPlayer.score > player.score) {
    return bestPlayer;
  }
  return player;
}, players[0]);

console.log(bestScorer);
```

---

## 🔹 Syntax বোঝা

* `bestPlayer` → এখন পর্যন্ত best scorer
* `player` → current player
* `players[0]` → প্রথম player দিয়ে শুরু

---

## 🔹 Logic

👉 compare করা হচ্ছে:

```
যার score বেশি → সে থাকবে
```

---

## 🔹 Step-by-step Table

| Step | bestPlayer | current player | compare | result |
| ---- | ---------- | -------------- | ------- | ------ |
| 1    | Jamal(88)  | Shekh(81)      | 88 > 81 | Jamal  |
| 2    | Jamal(88)  | Rakib(95)      | 95 > 88 | Rakib  |
| 3    | Rakib(95)  | Topu(91)       | 95 > 91 | Rakib  |
| 4    | Rakib(95)  | Sohel(72)      | 95 > 72 | Rakib  |

---

## ✅ Output

```js
{ name: "Rakib Hossain", score: 95 }
```

---

## 🔹 console.log(bestPlayer, player) কী দেখাবে?

প্রতিটা iteration এ current best আর নতুন player দেখাবে:

```js
{ name: "Jamal Bhuyan", score: 88 } { name: "Shekh Morsalin", score: 81 }
{ name: "Jamal Bhuyan", score: 88 } { name: "Rakib Hossain", score: 95 }
{ name: "Rakib Hossain", score: 95 } { name: "Topu Barman", score: 91 }
...
```

---

# 🔚 Final Summary

* `reduce()` দিয়ে:

  * subtotal (sum) করা যায় ✅
  * best/min/max element বের করা যায় ✅
* accumulator (`subtotal`, `bestPlayer`) প্রতি step এ update হয়
* শেষে একটা final result দেয়

---

চাও হলে আমি এটা **real e-commerce discount + tax example** দিয়ে আরো advanced ভাবে দেখাতে পারি 👍

