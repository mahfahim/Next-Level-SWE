## 🧠 4-9 Selection Sort (বাংলায় সহজ ব্যাখ্যা)

---

# ❓ Selection Sort কী?

Selection Sort হলো একটা sorting algorithm যেখানে:

👉 পুরো array থেকে সবচেয়ে ছোট (minimum) element খুঁজে
👉 সেটাকে সামনে এনে রাখা হয়
👉 তারপর বাকি অংশে আবার একই কাজ করা হয়

---

# 💡 সহজভাবে চিন্তা করো

ধরো তুমি line এ দাঁড়িয়ে আছো:

👉 সবচেয়ে ছোট মানুষটাকে খুঁজে সামনে এনে দাও
👉 তারপর বাকি লাইনে আবার একই কাজ করো

---

# 📥 Example Input

```text id="ss1"
[5, 3, 8, 4, 2]
```

---

# 🎯 Goal

👉 ছোট থেকে বড় order এ সাজানো

```text id="ss2"
[2, 3, 4, 5, 8]
```

---

# 🧾 Code (তোমারটা)

```js id="ss3"
const selectionSort = (array) => {
  for (let i = 0; i < array.length - 1; i++) {
    console.log("State of arr:", array);

    let minIndex = i;

    for (let j = i + 1; j < array.length; j++) {
      if (array[j] < array[minIndex]) {
        minIndex = j;
      }
    }

    if (minIndex !== i) {
      [array[i], array[minIndex]] = [array[minIndex], array[i]];
    }

    console.log(`After pass ${i + 1}`, array);
  }
};

selectionSort([5, 3, 8, 4, 2]);
```

---

# 🧠 Step by Step Explanation

---

## 🔹 Step 1

```text id="ss4"
[5, 3, 8, 4, 2]
```

👉 পুরো array থেকে সবচেয়ে ছোট = 2
👉 2 কে index 0 এ আনা হয়

```text id="ss5"
[2, 3, 8, 4, 5]
```

---

## 🔹 Step 2

বাকি অংশ:

```text id="ss6"
[3, 8, 4, 5]
```

👉 সবচেয়ে ছোট = 3
👉 already ঠিক জায়গায়

```text id="ss7"
[2, 3, 8, 4, 5]
```

---

## 🔹 Step 3

```text id="ss8"
[8, 4, 5]
```

👉 smallest = 4
👉 swap

```text id="ss9"
[2, 3, 4, 8, 5]
```

---

## 🔹 Step 4

```text id="ss10"
[8, 5]
```

👉 smallest = 5
👉 swap

```text id="ss11"
[2, 3, 4, 5, 8]
```

---

## 🔹 Final Result

```text id="ss12"
[2, 3, 4, 5, 8]
```

---

# 📊 Visual Idea (Flow)

```text id="ss13"
Step 1 → smallest pick → move to front
Step 2 → next smallest pick
Step 3 → repeat
Step 4 → sorted array
```

---

# ⚡ Complexity

| Type  | Value   |
| ----- | ------- |
| Time  | O(n²) ❌ |
| Space | O(1) ✔  |

---

# 🧠 কেন O(n²)?

👉 outer loop চলে n বার
👉 inner loop ও প্রায় n বার
👉 তাই n × n = n²

---

# 🎯 এক লাইনে মনে রাখো

👉 “প্রতিবার সবচেয়ে ছোট element খুঁজে সামনে এনে রাখাই Selection Sort”

---

চাও হলে আমি তোমাকে **Bubble Sort vs Selection Sort vs Insertion Sort** একসাথে compare করে খুব সহজভাবে বুঝিয়ে দিতে পারি।
