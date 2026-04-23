## 🧠 4-6 Binary Search Algorithm (বাংলায় সহজ ব্যাখ্যা)

---

# ❓ Binary Search কী?

Binary Search হলো একটা **fast searching technique** যেটা sorted array এর মধ্যে কোনো value খুঁজে বের করে।

👉 সাধারণভাবে খোঁজ করলে এক এক করে দেখতে হয় (slow)
👉 কিন্তু binary search এ প্রতি ধাপে অর্ধেক data বাদ দিয়ে দেই (fast)

---

# ⚠️ শর্ত

Binary Search কাজ করে শুধু তখনই যখন:

👉 array অবশ্যই sorted হতে হবে

---

# 📥 Example Input

```js id="bs1"
[3, 5, 6, 7, 9, 11], target = 7
```

---

# 🎯 Output

```js id="bs2"
3
```

কারণ `7` আছে index `3` এ।

---

# 💡 Idea (Core Logic)

আমরা ৩টা pointer ব্যবহার করি:

* `low` → শুরু
* `high` → শেষ
* `mid` → মাঝখান

---

# 🧾 Code

```js id="bs3"
const binarySearch = (arr, target) => {
  let low = 0;
  let high = arr.length - 1;

  while (low <= high) {
    const mid = Math.floor((low + high) / 2);
    const guess = arr[mid];

    if (guess === target) {
      return mid;
    } 
    else if (guess > target) {
      high = mid - 1;
    } 
    else {
      low = mid + 1;
    }
  }

  return -1;
};

console.log(binarySearch([3, 5, 6, 7, 9, 11], 7));
```

---

# 📊 Step by Step (Visual Explanation)

ধরি:

```text id="bs4"
Array: [3, 5, 6, 7, 9, 11]
Target: 7
```

---

## 🔹 Step 1

```text id="bs5"
low = 0
high = 5
mid = 2 → value = 6
```

👉 6 < 7
👉 তাই বাম বাদ → ডান দিকে যাবো

```text id="bs6"
low = mid + 1 → 3
```

---

## 🔹 Step 2

```text id="bs7"
low = 3
high = 5
mid = 4 → value = 9
```

👉 9 > 7
👉 তাই ডান বাদ → বাম দিকে যাবো

```text id="bs8"
high = mid - 1 → 3
```

---

## 🔹 Step 3

```text id="bs9"
low = 3
high = 3
mid = 3 → value = 7 ✔
```

👉 পাওয়া গেছে!

---

# 📤 Output

```text id="bs10"
3
```

---

# ⚡ কেন Binary Search fast?

| Method        | Time       |
| ------------- | ---------- |
| Linear Search | O(n) ❌     |
| Binary Search | O(log n) ✔ |

👉 প্রতি step এ half data বাদ হয়ে যায়

---

# 🧠 সহজভাবে মনে রাখো

👉 “মাঝখান দেখে decide করো left যাবে না right”

---

# 🎯 এক লাইনে summary

👉 “Sorted array এ target খুঁজতে প্রতি step এ half data বাদ দিয়ে search করাই Binary Search”

---

চাও হলে আমি এটা আরও easy করে **step-by-step animation style explanation** বা **recursive version** ও বুঝিয়ে দিতে পারি।

