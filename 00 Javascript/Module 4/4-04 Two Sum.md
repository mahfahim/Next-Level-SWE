এইটা একটা খুব important interview problem—**Two Sum**। আমি একদম সহজভাবে বাংলায় বুঝাই।

---

# 🧠 Problem কী বলছে?

তোমাকে দেওয়া আছে:

* একটা array: `numbers`
* একটা target value

👉 তোমাকে এমন 2টা number খুঁজে বের করতে হবে যাদের যোগফল = target

এবং তাদের **index return করতে হবে**

---

## 📥 Input

```js id="tw1"
[2, 11, 7, 15], target = 9
```

---

## 📤 Output

```js id="tw2"
[0, 2]
```

কারণ:

```js id="tw3"
numbers[0] = 2
numbers[2] = 7

2 + 7 = 9 ✔
```

---

# ⚠️ Constraint

* একই element দুবার use করা যাবে না
* exact 1টা solution থাকবে
* Time complexity হতে হবে O(n)

---

# 💡 Naive solution (slow)

সব pair check করলে:

* O(n²)

কিন্তু আমরা চাই fast solution

---

# 🚀 Efficient Solution: Map ব্যবহার

আমরা একটা `Map` ব্যবহার করব:

👉 আগে দেখা numbers store করব
👉 আর complement খুঁজব

---

# 🧠 Core Idea

ধরি:

```js id="tw4"
target = 9
current number = 2
```

তাহলে দরকার:

```js id="tw5"
9 - 2 = 7
```

👉 যদি 7 আগে থেকে দেখা থাকে → answer পেয়ে গেছি

---

# 🧾 Step by Step Flow

Array:

```js id="tw6"
[2, 11, 7, 15]
```

---

## Step 1: 2

* need = 9 - 2 = 7
* 7 নাই
* map এ store: 2 → index 0

---

## Step 2: 11

* need = 9 - 11 = -2
* নাই
* store: 11 → index 1

---

## Step 3: 7

* need = 9 - 7 = 2
* 2 আগে থেকেই আছে ✔

👉 answer পেয়ে গেলাম

---

# 📊 Text Drawing

```text
index:   0    1    2    3
value:   2   11    7   15

map:
2 → 0
11 → 1

current = 7
need = 2 ✔ (map এ আছে)
```

---

# 🧾 Code (O(n) solution)

```js id="tw7"
function twoSum(numbers, target) {
  const map = new Map();

  for (let i = 0; i < numbers.length; i++) {
    const current = numbers[i];
    const need = target - current;

    if (map.has(need)) {
      return [map.get(need), i];
    }

    map.set(current, i);
  }

  return undefined;
}
```

---

# 📤 Output

```js id="tw8"
console.log(twoSum([2, 11, 7, 15], 9));
// [0, 2]
```

---

# ⚡ Complexity

| Type  | Complexity |
| ----- | ---------- |
| Time  | O(n) ✔     |
| Space | O(n) ✔     |

---

# 🎯 কেন এই approach ভালো?

* একবারেই solve হয়
* nested loop লাগে না
* real-world scalable

---

# 🧠 এক লাইনে মনে রাখো

👉 “current number এর জন্য দরকারি complement আগে দেখা আছে কিনা Map দিয়ে check করো”

---

চাও হলে আমি next level এ:

* **3 Sum problem**
* **Two Sum sorted array (two pointers)**

ও সহজ করে বুঝিয়ে দিতে পারি।
