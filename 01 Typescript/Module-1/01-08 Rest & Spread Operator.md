ঠিক আছে, আমি তোমাকে **01-08 Rest & Spread Operator (TypeScript)** একদম শুরু থেকে advanced পর্যন্ত clean + easy tutorial দিচ্ছি 👇

---

# 🔰 01–08: Rest & Spread Operator (Full Tutorial)

👉 এখানে একটা জিনিস খুব গুরুত্বপূর্ণ:

> `...` এই তিনটা dot দুইভাবে use হয়

* Spread
* Rest

দেখতে এক, কিন্তু কাজ উল্টো 🔥

---

# 🔹 1. Spread Operator (Expand করা)

👉 মানে:

> একটা array বা object কে “ভেঙে” আলাদা আলাদা করে দেওয়া

---

## 🔥 Array Example

```ts id="s1"
const schoolFriends = ["Pintu", "Chintu", "Bulbul"];
const collegeFriends = ["Smart", "Very Smart"];
```

---

## 🔹 Spread ব্যবহার

```ts id="s2"
const friends = ["Rahim", "Karim"];

friends.push(...schoolFriends);
friends.push(...collegeFriends);
```

---

## 🧠 কী হচ্ছে এখানে?

```ts id="s3"
...schoolFriends
```

👉 মানে:

```txt id="s4"
"Pintu", "Chintu", "Bulbul"
```

👉 array ভেঙে আলাদা আলাদা element হয়ে গেছে

---

## 🟢 Output

```ts id="s5"
console.log(friends);
```

```json id="s6"
[
  "Rahim",
  "Karim",
  "Pintu",
  "Chintu",
  "Bulbul",
  "Smart",
  "Very Smart"
]
```

---

# 🔥 Spread in Object

```ts id="o1"
const user = { name: "Mezba", phone: "0170000000" };
const extra = { hobby: "outing", color: "black" };
```

---

## 🔹 Merge

```ts id="o2"
const userInfo = { ...user, ...extra };
```

---

## 🧠 কী হচ্ছে?

👉 দুইটা object merge হয়ে গেছে

---

## 🟢 Output

```json id="o3"
{
  "name": "Mezba",
  "phone": "0170000000",
  "hobby": "outing",
  "color": "black"
}
```

---

# 🔥 Spread meaning

👉 “খুলে দাও সব কিছু”

---

# 🔹 2. Rest Operator (Collect করা)

👉 মানে:

> অনেকগুলো value কে একসাথে array বানানো

---

## 🔥 Function Example

```ts id="r1"
const sendInvite = (...friends: string[]) => {
  console.log(friends);
};
```

---

## 🧠 এখানে কী হচ্ছে?

👉 সব arguments collect হয়ে array হয়ে যাচ্ছে

---

## 🔹 Call

```ts id="r2"
sendInvite("Pintu", "Chintu", "Bulbul");
```

---

## 🟢 Output

```id="r3"
["Pintu", "Chintu", "Bulbul"]
```

---

## 🔥 Real use

```ts id="r4"
const sendInvite = (...friends: string[]) => {
  friends.forEach((friend) =>
    console.log(`Send invite to ${friend}`)
  );
};
```

---

## 🟢 Output

```txt id="r5"
Send invite to Pintu
Send invite to Chintu
Send invite to Bulbul
```

---

# 🔥 Rest meaning

👉 “সব কিছু একসাথে জমা করো”

---

# 🔥 Spread vs Rest (Most Important)

| বিষয়   | Spread       | Rest               |
| ------ | ------------ | ------------------ |
| কাজ    | ভেঙে দেওয়া   | জড়ো করা            |
| use    | array/object | function parameter |
| চিন্তা | খুলে দাও     | জমাও               |

---

# 🧠 Easy Memory Trick

👉 `...` সামনে থাকলে → Spread (open)
👉 function parameter এ থাকলে → Rest (collect)

---

# 🔥 Real-life analogy

## 🟢 Spread

👉 ব্যাগ খুলে সব জিনিস বাইরে বের করে দিচ্ছো 🎒➡️📦📦📦

## 🔴 Rest

👉 অনেক জিনিস ব্যাগে ঢুকাচ্ছো 📦📦📦➡️🎒

---

# 🔥 Full Example (Combined)

```ts id="f1"
const school = ["A", "B"];
const college = ["C", "D"];

const all = ["X", ...school, ...college];

console.log(all);
```

---

## 🟢 Output

```id="f2"
["X", "A", "B", "C", "D"]
```

---

# 🔥 Object Spread Example

```ts id="f3"
const a = { x: 1 };
const b = { y: 2 };

const c = { ...a, ...b };

console.log(c);
```

---

## 🟢 Output

```id="f4"
{ x: 1, y: 2 }
```

---

# 🚀 Final Summary

👉 Spread:

* array/object ভেঙে দেয়
* merge করে

👉 Rest:

* অনেক value collect করে
* array বানায়

---

# 🧠 One-line memory

👉 Spread = open
👉 Rest = collect

---

চাও হলে আমি next এ তোমাকে **destructuring + spread + rest একসাথে interview tricky questions দিয়ে practice করাতে পারি 👍
