এই problemটা খুব classic একটা **Stack-based interview question** — নাম: *Valid Parentheses*।

আমি একদম সহজভাবে বুঝাই।

---

# 🧠 Problem কী বলছে?

তোমাকে একটা string দেওয়া আছে, যেমন:

```
"()[]{}"
"([{}])"
"(]"
"(()"
```

এখানে আছে ৩ ধরনের bracket:

* `()`
* `{}`
* `[]`

---

## 🎯 কাজ

তোমাকে বলতে হবে:

👉 এটা valid কিনা

---

# ✅ Valid হওয়ার নিয়ম

একটা string valid হবে যদি:

### 1️⃣ Open bracket অবশ্যই close হতে হবে

```
( → )
{ → }
[ → ]
```

### 2️⃣ Correct order হতে হবে

```
([{}]) ✅
```

কিন্তু:

```
([)] ❌
```

### 3️⃣ প্রতিটা closing bracket এর match থাকতে হবে

---

# 💡 কেন Stack ব্যবহার করি?

Stack হলো **Last In First Out (LIFO)**

👉 last open bracket আগে close হতে হবে

এটা bracket matching এর জন্য perfect

---

# 🧠 Idea

আমরা করব:

### 1️⃣ Open bracket পেলে stack এ push করব

### 2️⃣ Close bracket পেলে check করব stack এর top এর সাথে match করে কিনা

### 3️⃣ mismatch হলে → invalid

---

# 🚀 Step by Step Flow

ধরি input:

```
"([{}])"
```

---

## 📊 Text Drawing (Stack Process)

```
Start: stack = []

(   → push → [
[   → push → [(
{   → push → [({

}   → match with { → pop → [(

]   → match with [ → pop → [

)   → match with ( → pop → []

শেষে stack empty → VALID
```

---

# 🧾 Code (Stack ব্যবহার করে)

```js id="v7k2pq"
function isValid(s) {
  const stack = [];

  const map = {
    ")": "(",
    "}": "{",
    "]": "[",
  };

  for (let char of s) {
    // open bracket হলে push
    if (char === "(" || char === "{" || char === "[") {
      stack.push(char);
    } 
    // close bracket হলে check
    else {
      if (stack.length === 0) return false;

      const top = stack.pop();

      if (top !== map[char]) {
        return false;
      }
    }
  }

  return stack.length === 0;
}
```

---

# 📤 Output Examples

```js
console.log(isValid("()[]{}"));   // true
console.log(isValid("([{}])"));   // true
console.log(isValid("(]"));       // false
console.log(isValid("(()"));      // false
```

---

# ⚠️ কেন `(]` false?

```
(   → stack = [(]
]   → expected [ but got ( ❌
```

---

# ⚡ Time Complexity

* O(n) → প্রতিটা character একবার করে process হয়

# ⚡ Space Complexity

* O(n) → worst case stack এ সব open bracket থাকবে

---

# 🎯 এক লাইনে মনে রাখো

👉 “Open bracket push করো, close bracket এ match করো stack এর top এর সাথে”

---

চাও হলে আমি next level এ:

* **Trapping Rain Water (stack version)**
* বা **Next Greater Element**

এইগুলোও সহজ করে বুঝিয়ে দিতে পারি।
