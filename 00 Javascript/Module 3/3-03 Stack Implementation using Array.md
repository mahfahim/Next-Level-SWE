এখানে তুমি একটা **Stack (LIFO – Last In First Out)** structure implement করেছো array ব্যবহার করে। মানে যেটা শেষে ঢুকবে, সেটাই আগে বের হবে।

ধাপে ধাপে বুঝি:

---

## 🔹 Stack Class ব্যাখ্যা

```js
class Stack {
  constructor() {
    this.items = [];
  }
```

* `items` হচ্ছে একটা array যেখানে সব data রাখা হবে।

---

### 🔸 push(value) → O(1)

```js
push(value) {
  this.items.push(value);
}
```

* নতুন element stack-এর top এ যোগ করে।
* Example: `[10, 20]` → push(30) → `[10, 20, 30]`

---

### 🔸 pop() → O(1)

```js
pop() {
  if (this.isEmpty()) {
    return undefined;
  }
  return this.items.pop();
}
```

* stack-এর last element remove করে return করে।
* যদি empty হয় → `undefined`

---

### 🔸 peek() → O(1)

```js
peek() {
  if (this.isEmpty()) {
    return undefined;
  }
  return this.items[this.items.length - 1];
}
```

* শুধু last element দেখায়, remove করে না।

---

### 🔸 isEmpty() → O(1)

```js
isEmpty() {
  return this.items.length === 0;
}
```

* stack empty কিনা check করে।

---

### 🔸 print() → O(n)

```js
print() {
  console.log(this.items.slice().reverse().join(" -> "));
}
```

* stack কে top থেকে bottom পর্যন্ত print করে।
* reverse করা হয়েছে কারণ stack-এর top হচ্ছে last element।

---

## 🔹 Execution Step-by-Step

```js
const stack = new Stack();

stack.push(10);  // [10]
stack.push(20);  // [10, 20]
stack.push(30);  // [10, 20, 30]
```

এখন stack এর অবস্থা:

```
Top → 30
       20
       10
```

---

### 🔸 pop operations

```js
console.log(stack.pop()); // 30
```

* 30 বের হয়ে গেল
* stack: `[10, 20]`

```js
console.log(stack.pop()); // 20
```

* stack: `[10]`

```js
console.log(stack.pop()); // 10
```

* stack: `[]`

```js
console.log(stack.pop()); // undefined
```

* কারণ stack এখন empty

---

## 🔹 Final Output

```
30
20
10
undefined
```

---

## 🔹 মূল ধারণা (Key Idea)

* Stack সবসময় **Last In First Out (LIFO)**
* Array এর `push()` এবং `pop()` ব্যবহার করে সহজেই implement করা যায়
* সব operation প্রায় constant time `O(1)` except print

---

চাইলে আমি একই Stack টা real-life example (plate stack, browser history) দিয়ে আর clear করে দিতে পারি 👍
