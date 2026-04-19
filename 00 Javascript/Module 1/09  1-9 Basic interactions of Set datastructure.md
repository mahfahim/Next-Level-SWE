# Basic Interactions of Set in JavaScript (Bangla + Example + Output + Complexity)

Set হলো এমন একটা data structure যেখানে:
👉 duplicate value রাখা যায় না
👉 সব value unique থাকে
👉 fast lookup করা যায়

---

# ১. Set তৈরি + add()

```js id="set10"
let mySet = new Set();

mySet.add(10);
mySet.add(20);
mySet.add(20);
mySet.add(30);

console.log(mySet);
```

### Output:

```txt id="out10"
Set(3) { 10, 20, 30 }
```

## Complexity:

* add() → **O(1)** (average)

👉 কারণ Hash-based structure ব্যবহার করে

---

# ২. Array থেকে Set

```js id="set11"
let arr = [1, 2, 2, 3, 4, 4, 5];

let uniqueSet = new Set(arr);

console.log(uniqueSet);
```

### Output:

```txt id="out11"
Set(5) { 1, 2, 3, 4, 5 }
```

## Complexity:

* O(n) → পুরো array traverse করতে হয়

---

# ৩. Set.forEach()

```js id="set12"
let mySet = new Set([10, 20, 30]);

mySet.forEach((value) => {
  console.log(value);
});
```

### Output:

```txt id="out12"
10
20
30
```

## Complexity:

* O(n) → সব element visit করে

---

# ৪. Array.from(Set)

```js id="set13"
let mySet = new Set([1, 2, 3, 3, 4]);

let arr = Array.from(mySet);

console.log(arr);
```

### Output:

```txt id="out13"
[1, 2, 3, 4]
```

## Complexity:

* O(n) → সব element copy করতে হয়

---

# ৫. set.has()

```js id="set14"
let mySet = new Set([10, 20, 30]);

console.log(mySet.has(20));
console.log(mySet.has(100));
```

### Output:

```txt id="out14"
true
false
```

## Complexity:

* O(1) → average case

👉 hash lookup হয়

---

# ৬. set.delete()

```js id="set15"
let mySet = new Set([10, 20, 30]);

mySet.delete(20);

console.log(mySet);
```

### Output:

```txt id="out15"
Set(2) { 10, 30 }
```

## Complexity:

* O(1) → average case

---

# ৭. Remove Duplicate using Set

```js id="set16"
let arr = [1, 1, 2, 3, 3, 4];

let unique = [...new Set(arr)];

console.log(unique);
```

### Output:

```txt id="out16"
[1, 2, 3, 4]
```

## Complexity:

* O(n) → array traverse + insert into set

---

# ৮. includes() vs Set.has()

## Array.includes()

```js id="set17"
let arr = [10, 20, 30];

console.log(arr.includes(20));
```

### Output:

```txt id="out17"
true
```

## Complexity:

* O(n) → linear search

---

## Set.has()

```js id="set18"
let mySet = new Set([10, 20, 30]);

console.log(mySet.has(20));
```

### Output:

```txt id="out18"
true
```

## Complexity:

* O(1) → fast lookup

---

# ৯. Final Comparison Table

| Operation    | Example         | Complexity |
| ------------ | --------------- | ---------- |
| add()        | set.add(10)     | O(1)       |
| has()        | set.has(10)     | O(1)       |
| delete()     | set.delete(10)  | O(1)       |
| from array   | new Set(arr)    | O(n)       |
| forEach()    | set.forEach()   | O(n)       |
| Array.from() | Array.from(set) | O(n)       |
| includes()   | arr.includes(x) | O(n)       |

---

# Final Intuition

👉 Set ব্যবহার করা হয় যখন:

* duplicate remove করতে হবে
* fast search দরকার (O(1))
* unique data maintain করতে হবে

---

# One line summary

👉 “Set = unique data + fast lookup (O(1)) + efficient duplicate removal”

---

চাও হলে আমি next এ **Map vs Set vs Object real-world use case (cache, frequency counter)** খুব সহজভাবে explain করে দিতে পারি।
