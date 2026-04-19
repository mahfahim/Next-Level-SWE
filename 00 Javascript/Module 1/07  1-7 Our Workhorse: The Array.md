# Our Workhorse: JavaScript Array (Bangla + Example + Output)

JavaScript এ **Array** হলো সবচেয়ে বেশি ব্যবহার হওয়া data structure। এটা ordered list আকারে data রাখে এবং index দিয়ে access করা যায়।

---

# ১. Array Example

```js id="arr1"
let arr = [10, 20, 30, 40];
console.log(arr);
```

### Output:

```
[10, 20, 30, 40]
```

---

# ২. Direct Access → O(1)

👉 index দিয়ে সরাসরি data পাওয়া যায়

```js id="arr2"
let arr = [10, 20, 30, 40];

console.log(arr[2]);
```

### Output:

```
30
```

## ব্যাখ্যা:

* কোনো loop লাগে না
* সরাসরি memory থেকে access

---

# ৩. push() → O(1)

👉 শেষে element add করে

```js id="arr3"
let arr = [10, 20, 30];

arr.push(40);

console.log(arr);
```

### Output:

```
[10, 20, 30, 40]
```

---

# ৪. pop() → O(1)

👉 শেষে element remove করে

```js id="arr4"
let arr = [10, 20, 30, 40];

arr.pop();

console.log(arr);
```

### Output:

```
[10, 20, 30]
```

---

# ৫. shift() → O(n)

👉 প্রথম element remove করে, সব element shift হয়

```js id="arr5"
let arr = [10, 20, 30, 40];

arr.shift();

console.log(arr);
```

### Output:

```
[20, 30, 40]
```

## ব্যাখ্যা:

* 20, 30, 40 সবাই এক position left shift হয়
* তাই O(n)

---

# ৬. unshift() → O(n)

👉 শুরুতে element add করে

```js id="arr6"
let arr = [10, 20, 30];

arr.unshift(5);

console.log(arr);
```

### Output:

```
[5, 10, 20, 30]
```

## ব্যাখ্যা:

* সব element right shift হয়

---

# ৭. includes() → O(n)

👉 element আছে কিনা check করে

```js id="arr7"
let arr = [10, 20, 30, 40];

console.log(arr.includes(30));
```

### Output:

```
true
```

## ব্যাখ্যা:

* এক এক করে check করে
* worst case: সব element দেখতে হয়

---

# ৮. find() → O(n)

👉 condition match করে element খুঁজে

```js id="arr8"
let arr = [10, 20, 30, 40];

let result = arr.find(x => x > 25);

console.log(result);
```

### Output:

```
30
```

## ব্যাখ্যা:

* 10 → false
* 20 → false
* 30 → true → stop

---

# ৯. Time Complexity Summary

| Operation  | Example          | Complexity |
| ---------- | ---------------- | ---------- |
| arr[i]     | arr[2]           | O(1)       |
| push()     | add last         | O(1)       |
| pop()      | remove last      | O(1)       |
| shift()    | remove first     | O(n)       |
| unshift()  | add first        | O(n)       |
| includes() | search value     | O(n)       |
| find()     | condition search | O(n)       |

---

# Final Intuition

👉 Array খুব powerful কিন্তু mixed behavior:

* Fast → direct access, push, pop
* Slow → search, shift, unshift

---

চাও হলে আমি next এ **Stack & Queue (array দিয়ে JS implementation)** খুব সহজভাবে explain করতে পারি with real-life example (undo, task queue ইত্যাদি)।
