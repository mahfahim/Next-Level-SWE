এখানে তুমি **Linked List দিয়ে Queue implement করা** শিখছো।
এটা আগের array-based queue থেকে বেশি efficient।

---

## 🔹 Queue আবার কী?

👉 Queue কাজ করে **FIFO (First In First Out)** নিয়মে
মানে:

* আগে ঢুকলে আগে বের হবে

---

## 🔹 এখানে কী কী ব্যবহার করা হয়েছে?

```js
this.first  // queue এর সামনে (front)
this.last   // queue এর শেষ (back)
this.length // কয়টা element আছে
```

👉 Linked List ব্যবহার করার কারণে:

* সামনে remove → fast
* শেষে add → fast

---

## 🔹 Node class (recap)

```js
class Node {
  constructor(value) {
    this.value = value;
    this.next = null;
  }
}
```

👉 প্রতিটা node:

* value রাখে
* next দিয়ে পরের node কে point করে

---

## 🔹 Queue structure

```js
class Queue {
  constructor() {
    this.first = null;
    this.last = null;
    this.length = 0;
  }
}
```

👉 শুরুতে queue empty:

```text
first = null
last = null
length = 0
```

---

## 🔹 enqueue(value) → add (O(1))

```js
enqueue(value) {
  const newNode = new Node(value);

  if (this.isEmpty()) {
    this.first = newNode;
    this.last = newNode;
  } else {
    this.last.next = newNode;
    this.last = newNode;
  }

  this.length++;
}
```

### ✔️ Case 1: Queue empty

```text
enqueue(10)

10
first = last
```

---

### ✔️ Case 2: Queue already আছে

```text
enqueue(20)

10 → 20
first = 10
last = 20
```

👉 Step:

1. last.next → newNode
2. last update

---

## 🔹 dequeue() → remove (O(1))

```js
dequeue() {
  if (this.isEmpty()) {
    return undefined;
  }

  const nodeToRemove = this.first;

  if (this.first === this.last) {
    this.last = null;
  }

  this.first = this.first.next;
  this.length--;

  return nodeToRemove.value;
}
```

---

### 🔥 Step by step

ধরি:

```text
10 → 20 → 30
```

```js
dequeue()
```

👉 Step:

1. remove = 10
2. first = 20
3. return 10

👉 Result:

```text
20 → 30
```

---

### ✔️ Special case (1টা element)

```text
10
```

```js
dequeue()
```

👉 Result:

```text
empty queue
first = null
last = null
```

---

## 🔹 peek() → front দেখা (O(1))

```js
peek() {
  return this.first ? this.first.value : undefined;
}
```

👉 remove না করে সামনে কী আছে দেখায়

---

## 🔹 isEmpty()

```js
isEmpty() {
  return this.length === 0;
}
```

---

## 🔹 size()

```js
size() {
  return this.length;
}
```

---

## 🔹 print()

```js
print() {
  const array = [];
  let currentNode = this.first;

  while (currentNode) {
    array.push(currentNode.value);
    currentNode = currentNode.next;
  }

  console.log("Front -> " + array.join(" -> ") + " -> Back");
}
```

👉 traversal করে সব value দেখায়

---

## 🔹 Example Execution

```js
const q = new Queue();

q.enqueue(10);
q.enqueue(20);
q.enqueue(30);

q.print();
```

👉 Output:

```text
Front -> 10 -> 20 -> 30 -> Back
```

---

```js
q.dequeue();
q.print();
```

👉 Output:

```text
Front -> 20 -> 30 -> Back
```

---

## 🔥 কেন Linked List Queue better?

| Approach    | enqueue | dequeue |
| ----------- | ------- | ------- |
| Array       | O(1)    | O(n) ❌  |
| Linked List | O(1)    | O(1) ✅  |

👉 Array তে `shift()` লাগে → slow
👉 Linked List এ pointer change → fast

---

## ⚡ Summary

* Queue = FIFO
* first = front, last = back
* enqueue → শেষে add (O(1))
* dequeue → সামনে remove (O(1))
* Linked List ব্যবহার করলে সব operation fast হয়

---

চাও হলে next step এ আমি **Linked List দিয়ে Stack implementation (push/pop)** একইভাবে সহজ করে বুঝিয়ে দিতে পারি 👍
