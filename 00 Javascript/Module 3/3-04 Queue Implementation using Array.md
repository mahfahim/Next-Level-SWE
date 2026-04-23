এটা একটা **Queue (কিউ)** ডাটা স্ট্রাকচার, যেটা **FIFO (First In First Out)** নিয়মে কাজ করে।

মানে, যেটা আগে ঢুকবে সেটা আগে বের হবে।
একটা লাইনের কথা ভাবেন, যেমন টিকিট কাউন্টার—যে আগে লাইনে দাঁড়ায় সে আগে সার্ভিস পায়।

---

## 🔹 Code টা সহজভাবে বুঝি

### 1. constructor

```js
constructor() {
  this.items = [];
}
```

এখানে একটা খালি array নেওয়া হয়েছে, যেটার মধ্যে আমরা সব element রাখবো।

---

### 2. enqueue(value) → O(1)

```js
enqueue(value) {
  this.items.push(value);
}
```

নতুন value **শেষে add** করা হয়।

👉 উদাহরণ:

```
enqueue(10)
enqueue(20)

Queue: [10, 20]
```

---

### 3. dequeue() → O(n)

```js
dequeue() {
  if (this.isEmpty()) {
    return undefined;
  }

  return this.items.shift();
}
```

প্রথম element **remove** করা হয়।

⚠️ এখানে `shift()` ব্যবহার করা হয়েছে, তাই O(n)
কারণ সব element এক ধাপ করে shift হয়।

👉 উদাহরণ:

```
[10, 20, 30] → dequeue()

Result: 10 remove
Queue: [20, 30]
```

---

### 4. peek() → O(1)

```js
peek() {
  if (this.isEmpty()) {
    return undefined;
  }

  return this.items[0];
}
```

Queue এর প্রথম element দেখা হয় (remove না করে)।

👉 Output:

```
[10, 20, 30] → peek() = 10
```

---

### 5. isEmpty() → O(1)

```js
isEmpty() {
  return this.items.length === 0;
}
```

Queue খালি কিনা check করে।

---

### 6. print() → O(n)

```js
print() {
  console.log("Start -> ", this.items.join(" -> "), " -> end");
}
```

Queue কে সুন্দরভাবে print করে।

---

## 🔹 এখন নিচের execution টা দেখি

```js
const queue = new Queue();

queue.enqueue(10);
queue.enqueue(20);
queue.enqueue(30);
queue.print();
```

👉 Output:

```
Start ->  10 -> 20 -> 30  -> end
```

---

```js
queue.dequeue();
queue.print();
```

👉 Output:

```
Start ->  20 -> 30  -> end
```

---

## 🔥 Summary

* enqueue → শেষে add করে
* dequeue → সামনে থেকে remove করে
* FIFO follow করে
* `push()` fast (O(1))
* `shift()` slow (O(n)) ❗

---

## ⚠️ Important Note

এই array-based queue তে `dequeue()` slow হয় (O(n))
কারণ `shift()` use করা হচ্ছে।

👉 Better approach:

* **Linked List** ব্যবহার করলে dequeue O(1) করা যায়

---

আপনি চাইলে আমি next step হিসেবে **Linked List দিয়ে efficient Queue implementation** দেখাতে পারি 👍
