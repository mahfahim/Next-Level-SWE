```
// data setup

function generateSimData(size) {
  const itemPool = [
    "Apple",
    "Mango",
    "Banana",
    "Orange",
    "Grape",
    "Strawberry",
    "Pineapple",
    "Watermelon",
    "Cherry",
    "Blueberry",
    "Apple",
    "Orange",
  ];

  const generatedData = [];
  for (let i = 0; i < size; i++) {
    const randomIndex = Math.floor(Math.random() * itemPool.length);
    generatedData.push(itemPool[randomIndex]);
  }

  return generatedData;
}

const hugeDataSet = generateSimData(800000);

console.log("Data size", hugeDataSet.length);

// Brute Force

const arrStartTime = performance.now();

const removeDupArr = (arr) => {
  const newArr = [];

  arr.forEach((element) => {
    if (!newArr.includes(element)) {
      newArr.push(element);
    }
  });

  return newArr;
};
console.log(removeDupArr(hugeDataSet));

const arrEndTime = performance.now();

console.log(`Array Implementation took ${arrEndTime - arrStartTime}ms`);

// Set implementation

const setStartTime = performance.now();

const removeDupSet = (arr) => {
  const set = new Set(arr);

  return Array.from(set);
};

console.log(removeDupSet(hugeDataSet));

const setEndTime = performance.now();

console.log(`Set Implementation took ${setEndTime - setStartTime}ms`);

```

এই কোডটা মূলত দুইভাবে একটা বড় ডেটাসেট থেকে duplicate (একই মান) remove করার performance তুলনা করছে। নিচে ধাপে ধাপে বাংলায় বুঝিয়ে দিচ্ছি।

---

## 1. Data Setup (ডেটা তৈরি করা)

```js
function generateSimData(size)
```

এই ফাংশনটা একটা বড় array তৈরি করে।

### কী হচ্ছে এখানে:

* `itemPool` নামে একটা ছোট list আছে যেখানে কিছু ফলের নাম রাখা হয়েছে
* তারপর একটা loop চলে `size` বার
* প্রতিবার random একটা index বেছে নেয়
* সেই index এর ফল `generatedData` array তে push হয়

👉 ফলাফল:
একটা বড় array তৈরি হয় যেখানে একই item বারবার থাকতে পারে (duplicate থাকবে)

```js
const hugeDataSet = generateSimData(800000);
```

👉 এখানে 800,000 size এর একটা বড় dataset তৈরি করা হচ্ছে

---

## 2. Array (Brute Force) দিয়ে duplicate remove

```js
const removeDupArr = (arr) => {
  const newArr = [];

  arr.forEach((element) => {
    if (!newArr.includes(element)) {
      newArr.push(element);
    }
  });

  return newArr;
};
```

### কী হচ্ছে এখানে:

* একটা empty array `newArr` নেওয়া হয়েছে
* মূল array এর প্রতিটা element দেখা হচ্ছে
* যদি `newArr` এ আগে না থাকে → তাহলে push করা হচ্ছে
* যদি থাকে → ignore করা হচ্ছে

### সমস্যা (Performance issue):

```js
newArr.includes(element)
```

👉 এখানে প্রতিবার পুরো array scan করতে হয়
👉 তাই বড় data হলে খুব slow হয়ে যায়

### Time Complexity:

* O(n²) (খুব ধীর)

---

## 3. Set দিয়ে duplicate remove (Optimized way)

```js
const removeDupSet = (arr) => {
  const set = new Set(arr);
  return Array.from(set);
};
```

### কী হচ্ছে এখানে:

* `Set` automatically duplicate allow করে না
* তাই সব duplicate নিজে থেকেই বাদ পড়ে যায়
* তারপর আবার array তে convert করা হয়

### Advantage:

👉 খুব fast

### Time Complexity:

* O(n)

---

## 4. Performance Measure (সময় মাপা)

```js
performance.now()
```

এটা ব্যবহার করা হয়েছে:

* কোন method কত সময় নিচ্ছে সেটা measure করার জন্য

### Array method:

```js
arrStartTime → arrEndTime
```

### Set method:

```js
setStartTime → setEndTime
```

👉 শেষে দুইটার execution time compare করা হচ্ছে

---

## 5. Final Comparison (মূল কথা)

| Method           | Speed | Complexity | Best for |
| ---------------- | ----- | ---------- | -------- |
| Array + includes | Slow  | O(n²)      | ছোট data |
| Set              | Fast  | O(n)       | বড় data  |

---

## সহজভাবে বুঝলে

* Array method = প্রতিবার সবকিছু খুঁজে দেখে তারপর যোগ করে (slow)
* Set method = নিজেই duplicate handle করে ফেলে (fast)

---

## Bottom line

👉 বড় dataset (যেমন 800,000 items) হলে `Set` ব্যবহার করাই best choice
👉 Array method শুধুমাত্র ছোট data বা learning purpose এ ভালো

---

চাওলে আমি এর visual dry-run বা memory diagram দিয়ে আরও সহজ করে বুঝিয়ে দিতে পারি।
