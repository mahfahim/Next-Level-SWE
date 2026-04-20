### 2-3 Sorting এবং Flattening Array (JavaScript) — সহজভাবে ব্যাখ্যা

JavaScript এ array নিয়ে কাজ করার সময় দুইটা খুব গুরুত্বপূর্ণ অপারেশন হলো **sorting** এবং **flattening**। এগুলো ডেটা ম্যানিপুলেশনে খুব বেশি ব্যবহার হয়।

---

# 1. Sorting (সাজানো)

Sorting মানে হলো array এর উপাদানগুলোকে নির্দিষ্ট নিয়মে সাজানো।

## কীভাবে কাজ করে?

JavaScript এ sorting করা হয় `sort()` মেথড দিয়ে।

### Syntax:

```js
array.sort(compareFunction)
```

---

## Default behavior (গুরুত্বপূর্ণ)

```js
const arr = [10, 2, 30, 4];
arr.sort();
console.log(arr);
```

👉 Output:

```
[10, 2, 30, 4]
```

❗ সমস্যা:
Default `sort()` সংখ্যাকে সংখ্যা হিসেবে না নিয়ে string হিসেবে সাজায়।

---

## Correct way (number sorting)

### Ascending order (ছোট থেকে বড়)

```js
const arr = [10, 2, 30, 4];

arr.sort((a, b) => a - b);

console.log(arr);
```

👉 Output:

```
[2, 4, 10, 30]
```

---

### Descending order (বড় থেকে ছোট)

```js
arr.sort((a, b) => b - a);
```

👉 Output:

```
[30, 10, 4, 2]
```

---

## Word meaning (সহজভাবে)

* `a - b` → ছোট থেকে বড়
* `b - a` → বড় থেকে ছোট

👉 এখানে comparison function বলে দেয় কোনটা আগে যাবে।

---

# 2. Flattening Array (nested array ভাঙা)

Flattening মানে হলো nested array (array এর ভিতরে array) কে এক লেয়ারে নিয়ে আসা।

---

## Example (nested array)

```js
const arr = [1, [2, 3], [4, [5, 6]]];
```

এটা অনেকটা এমন:

```
[1,
  [2, 3],
  [4, [5, 6]]
]
```

---

## Flat করা (easy way)

### Syntax:

```js
array.flat(depth)
```

---

## 1 level flatten

```js
const arr = [1, [2, 3], [4, 5]];

console.log(arr.flat());
```

👉 Output:

```
[1, 2, 3, 4, 5]
```

---

## Deep flatten (nested বেশি হলে)

```js
const arr = [1, [2, [3, [4]]]];

console.log(arr.flat(2));
```

👉 Output:

```
[1, 2, 3, [4]]
```

---

## পুরো flatten (সব লেভেল ভাঙতে)

```js
console.log(arr.flat(Infinity));
```

👉 Output:

```
[1, 2, 3, 4]
```

---

## Word meaning (সহজভাবে)

* `flat()` → এক লেয়ার ভাঙে
* `flat(2)` → দুই লেয়ার পর্যন্ত ভাঙে
* `Infinity` → যত লেয়ার আছে সব ভাঙে

---

# Quick Summary

| Topic           | কাজ               |
| --------------- | ----------------- |
| sort()          | array কে সাজায়    |
| (a, b) => a - b | ascending         |
| (a, b) => b - a | descending        |
| flat()          | nested array ভাঙে |
| flat(Infinity)  | পুরো flatten করে  |

---

চাও হলে আমি next step এ **real interview question + trick based sorting/flattening problem** ও বুঝিয়ে দিতে পারি।

---
---
---

```
// Sort

const numbers = [40, 100, 1, 5, 25, 10];
const fruits = ["Banana", "apple", "Cherry", "date"];

fruits.sort((a, b) => a.localeCompare(b));

console.log(fruits);

// Nested array flat

const arr = [1, 2, 3, [4, 5, [6, 7, [8, 9, [10, 11]]]]];

const flatArr = arr.flat(Infinity);

console.log(flatArr);


const tagsFromPosts = [
  ["javascript", "react", "css"],
  ["node", "express"],
  ["css", "html", "react"],
];

const filterTags = [...new Set(tagsFromPosts.flat())];

console.log(filterTags);
```
