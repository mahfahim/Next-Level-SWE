## 2-10 Scenario Based Activity – Binning (Resampling) Time Series Data

### 📌 Binning কী?

Time Series Data মানে হলো সময় অনুযায়ী ডাটা (যেমন প্রতি মিনিট, ঘণ্টা, দিন অনুযায়ী ডাটা)।

**Binning (Resampling)** হলো এমন একটা প্রসেস যেখানে আমরা অনেকগুলো ছোট সময়ের ডাটাকে একসাথে করে বড় গ্রুপে (bin) রূপ দিই।

👉 সহজভাবে:

* প্রতি 1 মিনিটের ডাটা → 10 মিনিট বা 1 ঘণ্টার ডাটায় কনভার্ট করা
* ছোট ছোট ডাটা → average / sum / max / min করে summary বানানো

---

## 📊 Scenario

ধরি আমাদের কাছে প্রতি ঘণ্টার temperature data আছে, কিন্তু আমরা চাই **প্রতি 3 ঘণ্টার average temperature** বের করতে।

---

## 🧠 Input Data (Time Series)

```javascript
const temperatures = [
  { time: "10:00", temp: 30 },
  { time: "11:00", temp: 32 },
  { time: "12:00", temp: 31 },
  { time: "13:00", temp: 29 },
  { time: "14:00", temp: 28 },
  { time: "15:00", temp: 27 },
];
```

---

## ⚙️ Binning Logic (Resampling into groups of 3)

```javascript
const binSize = 3;
const result = [];

for (let i = 0; i < temperatures.length; i += binSize) {
  const chunk = temperatures.slice(i, i + binSize);

  const avgTemp =
    chunk.reduce((sum, item) => sum + item.temp, 0) / chunk.length;

  result.push({
    period: `${chunk[0].time} - ${chunk[chunk.length - 1].time}`,
    avgTemp: avgTemp,
  });
}

console.log(result);
```

---

## 🧾 Output

```javascript
[
  { period: "10:00 - 12:00", avgTemp: 31 },
  { period: "13:00 - 15:00", avgTemp: 28 }
]
```

---

## 🧩 সহজভাবে বোঝা

* প্রথম 3টা ডাটা → average = 31
* পরের 3টা ডাটা → average = 28
* তাই আমরা data কে “group” করে summary বানালাম

---

## 🎯 কেন Binning ব্যবহার করা হয়?

* Data clean & readable করতে
* Trend বুঝতে (noise কমাতে)
* Large dataset সহজে analyze করতে
* Dashboard বা chart বানাতে

---

চাও তাহলে আমি এটা **pandas (Python) দিয়ে real resampling example** বা **graph সহ visualization** করে দেখাতে পারি।


---
---
---

## 📌 Binning (Resampling) Time Series Data — Bangla Explanation

### 🧠 Scenario বোঝা

আমাদের কাছে কিছু **user events (click, scroll)** আছে, যেগুলো বিভিন্ন সময়ে ঘটেছে।

👉 সমস্যা:
আমরা জানতে চাই প্রতি **30 মিনিটে কতগুলো event হয়েছে**

এটাই হলো **Binning / Resampling**
মানে ছোট ছোট time event গুলোকে নির্দিষ্ট interval (bin) এ ভাগ করে count করা।

---

## 📥 Input Data

```javascript id="bin1"
const events = [
  { timestamp: "2025-10-22T10:01:00Z", type: "click" },
  { timestamp: "2025-10-22T10:05:00Z", type: "scroll" },
  { timestamp: "2025-10-22T10:14:00Z", type: "click" },
  { timestamp: "2025-10-22T10:31:00Z", type: "click" },
  { timestamp: "2025-10-22T10:45:00Z", type: "scroll" },
  { timestamp: "2025-10-22T11:02:00Z", type: "click" },
];
```

---

## ⚙️ Logic (30-minute bin তৈরি করা)

আমরা প্রতিটা timestamp কে নিচের মতো করে 30-minute interval এ নামাবো:

* 10:01 → 10:00 bin
* 10:31 → 10:30 bin
* 11:02 → 11:00 bin

---

## 💻 JavaScript Syntax (Binning Implementation)

```javascript id="bin2"
const binSize = 30 * 60 * 1000; // 30 minutes in milliseconds

const binnedEvents = {};

events.forEach((event) => {
  const date = new Date(event.timestamp);

  // বর্তমান time কে milliseconds এ নিয়ে bin calculate করা
  const time = date.getTime();
  const binTime = Math.floor(time / binSize) * binSize;

  const binKey = new Date(binTime).toISOString();

  if (!binnedEvents[binKey]) {
    binnedEvents[binKey] = { total: 0 };
  }

  binnedEvents[binKey].total += 1;
});

console.log(binnedEvents);
```

---

## 📤 Output

```javascript id="bin3"
{
  "2025-10-22T10:00:00.000Z": { total: 3 },
  "2025-10-22T10:30:00.000Z": { total: 2 },
  "2025-10-22T11:00:00.000Z": { total: 1 }
}
```

---

## 🧩 সহজভাবে বোঝা

### 🕒 10:00–10:30 bin

* 10:01 (click)
* 10:05 (scroll)
* 10:14 (click)

👉 মোট = 3

---

### 🕒 10:30–11:00 bin

* 10:31 (click)
* 10:45 (scroll)

👉 মোট = 2

---

### 🕒 11:00–11:30 bin

* 11:02 (click)

👉 মোট = 1

---

## 🎯 কেন এটা গুরুত্বপূর্ণ?

* User engagement analysis
* Website traffic pattern বোঝা
* Dashboard metrics তৈরি
* Time-based aggregation সহজ করা

---

## 🧠 Key Idea (এক লাইনে)

👉 Binning মানে হলো **time series data কে fixed interval এ group করে summary বানানো (count, sum, avg ইত্যাদি)**

---

চাও তাহলে আমি এটা **graph আকারে বা real dashboard style visualization** করেও বুঝিয়ে দিতে পারি।
