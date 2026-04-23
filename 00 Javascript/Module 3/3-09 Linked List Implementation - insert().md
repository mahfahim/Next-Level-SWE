এখানে মূল বিষয় হলো **`insert(index, value)`** — মানে Linked List এর **যেকোনো position-এ নতুন node ঢোকানো**।

তুমি ইতিমধ্যে `append` (শেষে) আর `prepend` (শুরুতে) জানো।
`insert()` এই দুইটার general version — যেকোনো জায়গায় add করতে পারে।

---

## 🔹 insert(index, value) কী করে?

👉 নির্দিষ্ট index এ নতুন node বসায়
👉 পুরনো node গুলোকে shift না করে, শুধু **link change করে**

---

## 🔹 Step 1: invalid index check

```js
if (index < 0 || index > this.length) {
  console.error("Index out of bound");
  return undefined;
}
```

👉 index যদি limit এর বাইরে হয় → error

---

## 🔹 Step 2: special case handle

### ✔️ শুরুতে insert

```js
if (index === 0) {
  return this.prepend(value);
}
```

👉 এটা prepend দিয়ে handle করা হচ্ছে

---

### ✔️ শেষে insert

```js
if (index === this.length) {
  return this.append(value);
}
```

👉 এটা append দিয়ে handle

---

## 🔹 Step 3: middle-এ insert (main logic)

এটাই আসল part 👇

```js
const leadingNode = this._traverseToIndex(index - 1);
const holdingNode = leadingNode.next;

const newNode = new Node(value);

leadingNode.next = newNode;
newNode.next = holdingNode;
```

---

## 🔥 এটা খুব গুরুত্বপূর্ণ — ধাপে ধাপে বুঝি

ধরি current list:

```
A → B → C → D → null
```

এখন তুমি insert করতে চাও:

```js
insert(2, "X")
```

👉 মানে index 2 তে "X" বসবে

---

### Step 1: leadingNode খোঁজা

```js
const leadingNode = this._traverseToIndex(index - 1);
```

👉 index - 1 = 1
👉 leadingNode = "B"

---

### Step 2: holdingNode রাখা

```js
const holdingNode = leadingNode.next;
```

👉 holdingNode = "C"

---

### Step 3: নতুন node create

```js
const newNode = new Node("X");
```

---

### Step 4: link change

```js
leadingNode.next = newNode;
newNode.next = holdingNode;
```

👉 এখন chain:

```
A → B → X → C → D → null
```

---

## 🔹 Helper method (_traverseToIndex)

```js
_traverseToIndex(index) {
  let count = 0;
  let currentNode = this.head;

  while (count !== index) {
    currentNode = currentNode.next;
    count++;
  }

  return currentNode;
}
```

👉 এটা head থেকে loop করে নির্দিষ্ট index এর node বের করে

---

## 🔹 Time Complexity

| Operation | Complexity |
| --------- | ---------- |
| insert    | O(n)       |

👉 কারণ:

* index পর্যন্ত traverse করতে হয়

---

## 🔹 ছোট example

```js
const list = new LinkedList();

list.append("A");
list.append("B");
list.append("C");

list.insert(1, "X");

list.print();
```

👉 Output:

```
A → X → B → C → null
```

---

## 🔥 Important Insight

* Array এর মতো shift করতে হয় না
* শুধু pointer change করলেই insert হয়ে যায়
* কিন্তু position খুঁজতে O(n) লাগে

---

## ⚠️ তোমার code এ ছোট issue

```js
linkedList.remove(0);
```

👉 কিন্তু `remove()` এখনো implement করা হয়নি
তাই এটা error দেবে

---

## ⚡ Summary

* insert = যেকোনো index এ add
* 3 case handle:

  * start → prepend
  * end → append
  * middle → pointer change
* helper function দিয়ে node খুঁজে আনা হয়
* time complexity = O(n)

---

চাও হলে next step এ আমি `remove()` method টা clean logic + diagram দিয়ে বুঝিয়ে দিতে পারি 👍
