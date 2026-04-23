এখানে তুমি **Linked List** এর একটা basic class দেখছো, যেখানে দুইটা গুরুত্বপূর্ণ method আছে:
👉 `append()` (শেষে add করা)
👉 `print()` (পুরো list দেখানো)

চল ধাপে ধাপে বুঝি।

---

## 🔹 Node recap

```js
class Node {
  constructor(value) {
    this.value = value;
    this.next = null;
  }
}
```

👉 প্রতিটা Node রাখে:

* `value` → data
* `next` → পরের Node-এর link

---

## 🔹 LinkedList class structure

```js
class LinkedList {
  constructor() {
    this.head = null;
    this.tail = null;
    this.length = 0;
  }
}
```

👉 এখানে:

* `head` → প্রথম Node
* `tail` → শেষ Node (important for fast append)
* `length` → মোট কয়টা node আছে

---

## 🔹 append(value) → সবচেয়ে গুরুত্বপূর্ণ

```js
append(value) {
  const newNode = new Node(value);
```

👉 নতুন Node তৈরি করা হলো

---

### ✅ Case 1: List খালি

```js
if (this.head === null) {
  this.head = newNode;
  this.tail = newNode;
}
```

👉 যদি list খালি থাকে:

* head = newNode
* tail = newNode

👉 Example:

```js
append(10)

10 → null
```

---

### ✅ Case 2: List already আছে

```js
else {
  this.tail.next = newNode;
  this.tail = newNode;
}
```

👉 এখানে:

1. আগের last node (`tail`) এর next → newNode
2. তারপর tail update করা হয়

👉 Example:

```js
append(20)

10 → 20 → null
```

---

### 🔹 length update

```js
this.length++;
```

👉 প্রতিবার নতুন node add হলে length বাড়ে

---

### 🔹 return this

```js
return this;
```

👉 chaining করার জন্য (optional but useful)

---

## 🔹 append execution example

```js
const list = new LinkedList();

list.append(10);
list.append(20);
list.append(30);
```

👉 Final structure:

```js
10 → 20 → 30 → null
```

---

## 🔹 print() method

```js
print() {
  const arr = [];
  let currentNode = this.head;

  while (currentNode !== null) {
    arr.push(currentNode.value);
    currentNode = currentNode.next;
  }

  console.log(arr.join(" -> "), "-> null ");
}
```

### কিভাবে কাজ করে:

1. `currentNode = head` থেকে শুরু
2. যতক্ষণ node আছে (`!== null`)
3. value array-তে push করে
4. next node এ যায়

---

### 👉 Example Output

```js
list.print();
```

👉 Output:

```js
10 -> 20 -> 30 -> null
```

---

## 🔥 কেন tail রাখা হয়?

যদি tail না রাখো:

* append করতে হলে প্রতি বার head থেকে loop করতে হবে ❌ (O(n))

tail থাকলে:

* সরাসরি last node এ access পাওয়া যায় ✅ (O(1))

---

## ⚡ Summary

* `append()` → list এর শেষে node add করে
* empty হলে head & tail একই হয়
* না হলে tail দিয়ে দ্রুত add করা হয়
* `print()` → traversal করে সব value দেখায়
* Linked List traversal সবসময় head থেকে শুরু হয়

---

চাও হলে next step এ আমি `prepend`, `insert`, `remove` খুব clean ভাবে example সহ বুঝিয়ে দিতে পারি 👍
