JavaScript-এ `Object` আর `Map`—দুটাই key-value data store করার জন্য ব্যবহার হয়। কিন্তু `Object` এর কিছু limitation আছে, যেগুলোর জন্য `Map` introduce করা হয়েছে।

চল সহজভাবে বুঝি 👇

---

# 🔴 Object এর limitation (সমস্যা)

### 1. Key সবসময় string হয়ে যায়

```js
const obj = {};

const key1 = { id: 1 };
const key2 = { id: 2 };

obj[key1] = "value1";
obj[key2] = "value2";

console.log(obj);
```

👉 Output:

```js
{ "[object Object]": "value2" }
```

**কেন এমন হলো?**

* Object key হিসেবে non-string দিলে JS সেটাকে string বানিয়ে নেয়
* `{}` → `"[object Object]"` হয়ে যায়
* তাই key clash হয় (একই হয়ে যায়)

---

### 2. Key হিসেবে object safely ব্যবহার করা যায় না

* তুমি আলাদা object দিয়েও unique key রাখতে পারো না
* সব key শেষমেশ string হয়ে conflict করে

---

### 3. Order guarantee থাকে না (পুরোপুরি reliable না)

* Object-এ insertion order কখনো কখনো maintain হয়, কিন্তু সব ক্ষেত্রে predictable না

---

### 4. Built-in properties conflict করতে পারে

```js
const obj = {};

obj["toString"] = "hello";
```

👉 `toString` আগে থেকেই Object-এর method

* তাই conflict হতে পারে

---

### 5. Size বের করা ঝামেলার

```js
Object.keys(obj).length
```

👉 direct কোনো `.size` property নাই

---

---

# 🟢 Map কি? (Introduction)

`Map` হলো JavaScript-এর একটি built-in data structure
যেখানে তুমি **any type key** use করতে পারো (object সহ)

```js
const map = new Map();
```

---

# 🟢 Map এর সুবিধা

### 1. Any type key support করে

```js
const map = new Map();

const key1 = { id: 1 };
const key2 = { id: 2 };

map.set(key1, "value1");
map.set(key2, "value2");

console.log(map);
```

👉 এখানে key1 আর key2 আলাদা থাকবে
✔️ কোনো conflict নাই

---

### 2. Order maintain করে

* যেভাবে insert করবে, সেভাবেই থাকবে

---

### 3. Built-in method আছে

```js
map.set("name", "Fahim");   // add
map.get("name");            // get
map.has("name");            // check
map.delete("name");         // remove
```

---

### 4. Size খুব সহজে পাওয়া যায়

```js
map.size
```

---

### 5. Iteration সহজ

```js
for (const [key, value] of map) {
  console.log(key, value);
}
```

---

---

# 🔄 Object vs Map (short comparison)

| Feature         | Object ❌         | Map ✅                 |
| --------------- | ---------------- | --------------------- |
| Key type        | string only      | any type              |
| Order           | unreliable       | guaranteed            |
| Size            | manual           | `.size`               |
| Performance     | smaller use case | better for large data |
| Built-in method | কম               | বেশি                  |

---

# 🎯 কখন কোনটা ব্যবহার করবে?

👉 Object use করো:

* simple data (API response, config)
* key string হলে

👉 Map use করো:

* dynamic key (object, function)
* large data
* order maintain দরকার হলে

---

চাইলে আমি Object vs Map এর **real life analogy** বা **interview style প্রশ্ন** দিয়েও বুঝিয়ে দিতে পারি।

---
---
---
---

```
// const obj = {};

// const course2 = "math101";
// const course1 = "history101";

// obj[course2] = { courseId: "level2" };
// obj[course1] = { courseId: "level1" };

// console.log(obj); 
// Output: { math101: { courseId: 'level2' }, history101: { courseId: 'level1' } }

///////////////////////////////////////////

const course1 = { name: "Programming Hero" };
const course2 = { name: "Next Level Web Development" };

const courses = [
  [course1, "Level1"],
  [course2, "Level2"],
];




////////////////////////////////////////////

const map = new Map(courses);

console.log(map);

map.set(course1, { courseId: "level1" });
map.set(course2, { courseId: "level2" });

console.log(map);

map.forEach((value, key) => (key.name = "Shohoz Shorol Simple " + key.name));

console.log(map);

for (let key of map.keys()) {
  key.name = "Shohoz Shorol Simple " + key.name;
}

console.log(map);
```
