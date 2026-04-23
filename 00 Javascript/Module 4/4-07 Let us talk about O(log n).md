## 🧠 4-7 O(log n) কী? (Binary Search দিয়ে সহজ ব্যাখ্যা)

এই টপিকটা বুঝতে হলে আগে একটা কথা খুব পরিষ্কার হতে হবে:

👉 Binary Search শুধু “fast search” না
👉 এটা দেখায় **কিভাবে সমস্যা প্রতি step এ ছোট হতে থাকে**

---

# ❓ O(log n) মানে কী?

👉 O(log n) মানে হলো:
প্রতিবার কাজ করলে problem size **আধা (half)** হয়ে যায়

---

## 📌 সহজ উদাহরণ

ধরো তোমার কাছে 8টা number আছে:

```text id="lg1"
[1, 2, 3, 4, 5, 6, 7, 8]
```

Binary search করলে:

* 8 → 4 → 2 → 1

👉 মাত্র 3 step এ শেষ

---

# 🧠 মূল ধারণা

👉 প্রতি step এ data half হয়ে যাচ্ছে
👉 তাই খুব দ্রুত শেষ হয়ে যায়

---

# 🚀 তোমার code কী করছে (Step-by-step)

তুমি যে code দিয়েছো সেটা শুধু search না, বরং **step-by-step log দেখায়** যাতে বুঝতে পারো O(log n) কীভাবে কাজ করে।

---

# 📊 উদাহরণ 1: target = 7

```text id="lg2"
[3, 5, 6, 7, 9, 11]
```

---

## 🔹 Step 1

```text id="lg3"
Window: [3, 5, 6, 7, 9, 11]
mid = 6
6 < 7 → left বাদ, right নাও
```

👉 এখন array half হয়ে গেল:

```text id="lg4"
[7, 9, 11]
```

---

## 🔹 Step 2

```text id="lg5"
mid = 9
9 > 7 → right বাদ, left নাও
```

👉 আবার half:

```text id="lg6"
[7]
```

---

## 🔹 Step 3

```text id="lg7"
mid = 7 → FOUND ✔
```

---

# 📊 উদাহরণ 2: target = 3 (not found)

```text id="lg8"
[2, 4, 6, 8, 10, 12]
```

👉 প্রতিবার half হবে:

```text id="lg9"
Step 1: [2, 4, 6, 8, 10, 12]
Step 2: [2, 4]
Step 3: [2]
Step 4: ❌ stop (not found)
```

---

# ⚡ কেন O(log n) এত fast?

ধরো:

| n (size)  | steps |
| --------- | ----- |
| 8         | 3     |
| 16        | 4     |
| 32        | 5     |
| 1,000,000 | 20    |

👉 বিশাল data হলেও step খুব কম

---

# 📉 Linear Search vs Binary Search

| Method        | Time       |
| ------------- | ---------- |
| Linear Search | O(n) ❌     |
| Binary Search | O(log n) ✔ |

---

# 🧠 সহজভাবে মনে রাখো

👉 “প্রতিবার data কে half করে ফেললে সেটাই O(log n)”

---

# 🎯 এক লাইনে summary

👉 Binary search হলো এমন algorithm যেখানে প্রতিটা step এ search space অর্ধেক হয়ে যায়, আর সেটাই O(log n)

---

চাও হলে আমি তোমাকে এটা **tree diagram দিয়ে visual explain** করে দিতে পারি, তাহলে O(log n) আরও clear হয়ে যাবে।
