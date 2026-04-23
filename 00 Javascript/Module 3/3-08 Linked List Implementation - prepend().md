এখানে মূল ফোকাস হলো **`prepend()` method**—মানে Linked List এর **শুরুর দিকে (head এ) নতুন node add করা**।

চল সহজভাবে বুঝি।

---

## 🔹 prepend(value) কী করে?

👉 `append()` যেমন শেষে add করে
👉 `prepend()` তেমন **শুরুতে add করে**

---

## 🔹 কোডটা দেখি

```js
prepend(value) {
  const newNode = new Node(value);
```

👉 প্রথমে নতুন একটা node তৈরি করা হলো

---

### ✅ Case 1: List খালি

```js
if (this.head === null) {
  this.head = newNode;
  this.tail = newNode;
}
```

👉 যদি list একদম empty হয়:

* head = newNode
* tail = newNode

👉 Example:

```js
prepend(10)

10 → null
```

---

### ✅ Case 2: List already আছে

```js
else {
  newNode.next = this.head;
  this.head = newNode;
}
```

👉 এখানে 2টা important step:

1. `newNode.next = this.head`
   → নতুন node পুরনো head কে point করছে

2. `this.head = newNode`
   → এখন newNode-ই নতুন head

---

## 🔹 Example দিয়ে বুঝি

ধরি current list:

```js
10 → 20 → 30 → null
```

এখন:

```js
prepend(5)
```

### Step:

1. newNode = 5
2. newNode.next = 10
3. head = 5

👉 Final:

```js
5 → 10 → 20 → 30 → null
```

---

## 🔹 length update

```js
this.length++;
```

👉 node add হলে count বাড়ে

---

## 🔹 return this

```js
return this;
```

👉 chaining এর জন্য (optional)

---

## 🔹 prepend vs append

| Method  | কোথায় add করে | Time Complexity |
| ------- | ------------- | --------------- |
| append  | শেষে          | O(1)            |
| prepend | শুরুতে        | O(1)            |

👉 prepend fast কারণ:

* সরাসরি head change হয়
* loop লাগে না

---

## 🔹 ছোট execution example

```js
const list = new LinkedList();

list.append(10);
list.append(20);

list.prepend(5);

list.print();
```

👉 Output:

```js
5 -> 10 -> 20 -> null
```

---

## 🔥 Important Insight

* prepend এ শুধু pointer change হয়
* কোন node shift করতে হয় না
* তাই এটা খুব efficient (O(1))

---

## ⚡ Summary

* prepend = শুরুতে add
* newNode → old head কে point করে
* head update হয়
* empty হলে head & tail same হয়
* সবসময় O(1)

---

চাও হলে next step এ `insert()` method টা পুরোটা complete করে clean logic সহ বুঝিয়ে দিতে পারি 👍
