চল পুরো কোডটা দুইভাবে বুঝি:
১) লাইন বাই লাইন বাংলা কমেন্ট
২) তারপর word-by-word + syntax breakdown

---

## ১. বাংলা কমেন্টসহ ব্যাখ্যা

```js
// const obj = {};

// obj[course2] = { courseId: "level2" };
// obj[course1] = { courseId: "level1" };

// console.log(obj);

// উপরের অংশটা Object দিয়ে key-value store করার চেষ্টা
// কিন্তু object কে key হিসেবে দিলে সেটা string হয়ে যায়
// তাই এখানে expected result পাওয়া যায় না


// দুইটা object তৈরি করা হচ্ছে
const course1 = { name: "Programming Hero" };
const course2 = { name: "Next Level Web Development" };


// একটা array তৈরি করা হচ্ছে
// এখানে প্রতিটা item হচ্ছে [key, value] pair
const courses = [
  [course1, "Level1"],
  [course2, "Level2"],
];


// Map তৈরি করা হচ্ছে
// courses array থেকে Map auto key-value বানিয়ে নেয়
const map = new Map(courses);


// নিচেরগুলো alternative way (comment করা আছে)

// map.set(course1, { courseId: "level1" });
// -> course1 কে key ধরে নতুন value set করা

// map.set(course2, { courseId: "level2" });


// map.forEach((value, key) => (key.name = "Shohoz Shorol Simple " + key.name));
// -> map এর প্রতিটা item loop করে
// -> key (object) এর name property modify করা হচ্ছে


// for (let key of map.keys()) {
//   key.name = "Shohoz Shorol Simple " + key.name;
// }
// -> map.keys() দিয়ে সব key পাওয়া যায়
// -> প্রতিটা key (object) modify করা হচ্ছে


// final output দেখানো হচ্ছে
console.log(map);
```

---

## ২. Word by Word + Syntax Explanation

### ▶️ object declaration

```js
const course1 = { name: "Programming Hero" };
```

* `const` → variable declare (reassign করা যাবে না)
* `course1` → variable নাম
* `=` → assignment operator
* `{}` → object literal
* `name:` → key (property name)
* `"Programming Hero"` → value (string)

---

### ▶️ array of pairs

```js
const courses = [
  [course1, "Level1"],
  [course2, "Level2"],
];
```

* `[]` → array
* `[course1, "Level1"]` → nested array (pair)

  * index 0 = key
  * index 1 = value
* এই structure Map constructor এর জন্য special format

---

### ▶️ Map creation

```js
const map = new Map(courses);
```

* `new` → নতুন instance তৈরি
* `Map` → built-in data structure
* `()` → constructor call
* `courses` → iterable (array of pairs)

👉 internally এটা এমন কাজ করে:

```js
map.set(course1, "Level1");
map.set(course2, "Level2");
```

---

### ▶️ map.set()

```js
map.set(course1, { courseId: "level1" });
```

* `map` → Map instance
* `.set()` → method (value set করার জন্য)
* `course1` → key
* `{ courseId: "level1" }` → value

---

### ▶️ map.forEach()

```js
map.forEach((value, key) => ...)
```

* `forEach` → loop method
* `(value, key)` → callback parameters

  * value আগে
  * key পরে (important difference from object)

---

### ▶️ arrow function

```js
(value, key) => (key.name = ...)
```

* `=>` → arrow function
* `( )` → parameters
* right side expression auto return হয়

---

### ▶️ map.keys()

```js
for (let key of map.keys())
```

* `for...of` → iterable loop
* `map.keys()` → সব key return করে (iterator)
* `key` → প্রতিটা iteration এ current key

---

### ▶️ property modify

```js
key.name = "Shohoz Shorol Simple " + key.name;
```

* `key.name` → object এর property access
* `=` → assign new value
* `"text" + key.name` → string concatenate

👉 important:

* `key` object reference
* তাই modify করলে original object change হয়

---

### ▶️ console.log

```js
console.log(map);
```

* `console` → browser/node console object
* `.log()` → output দেখানোর method
* `map` → পুরো Map print হবে

---

## 🔥 Key Insight (সবচেয়ে গুরুত্বপূর্ণ)

* Object → key string হয়ে যায়
* Map → key original type (object সহ)

👉 তাই:

```js
map.set(course1, "Level1");
```

এটা safe

কিন্তু:

```js
obj[course1] = "Level1";
```

এটা problematic

---

চাওলে আমি Object vs Map side-by-side example দিয়ে difference টা output সহ দেখাতে পারি।

