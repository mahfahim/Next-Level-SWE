ঠিক আছে, আমি তোমার পুরো টিউটোরিয়ালটা **console সহ clean + correct করে update করে দিলাম** 👇

---

# 🔰 01–09: Destructuring in TypeScript (Full Tutorial Updated)

## 🧠 Destructuring কী?

👉 Destructuring মানে:

> object বা array থেকে data সহজভাবে বের করে নেওয়া

---

# 🔹 1. Object Destructuring

```ts id="u1"
const user = {
  id: 1,
  name: "Fahim",
  age: 25,
};
```

---

## ❌ Old way

```ts id="u2"
const name = user.name;
const age = user.age;
```

---

## ✅ New way

```ts id="u3"
const { name, age } = user;

console.log(name);
console.log(age);
```

---

## 🟢 Output

```txt id="u4"
Fahim
25
```

---

# 🔹 2. Rename in Object Destructuring

```ts id="r1"
const { name: userName } = user;

console.log(userName);
```

---

## 🟢 Output

```txt id="r2"
Fahim
```

---

# 🧠 মানে

* `name` → original property
* `userName` → নতুন variable name

---

# 🔹 3. Nested Object Destructuring

```ts id="n1"
const user = {
  id: 123,
  name: {
    firstName: "Mezbaul",
    middleName: "Abedin",
    lastName: "Forhan",
  },
};
```

---

## 🔥 Destructuring

```ts id="n2"
const {
  name: { middleName },
} = user;

console.log(middleName);
```

---

## 🟢 Output

```txt id="n3"
Abedin
```

---

# 🔹 4. Array Destructuring

```ts id="a1"
const friends = ["Karim", "Rahim", "Mahim"];

const [first, second, third] = friends;

console.log(first);
console.log(second);
console.log(third);
```

---

## 🟢 Output

```txt id="a2"
Karim
Rahim
Mahim
```

---

# 🔹 5. Skip Elements

```ts id="s1"
const [, , bestFriend] = friends;

console.log(bestFriend);
```

---

## 🟢 Output

```txt id="s2"
Mahim
```

---

# 🔹 6. Default Value

```ts id="d1"
const user2 = { name: "Fahim" };

const { country = "Bangladesh" } = user2;

console.log(country);
```

---

## 🟢 Output

```txt id="d2"
Bangladesh
```

---

# 🔹 7. Function Parameter Destructuring

```ts id="f1"
function printUser({ name, age }: { name: string; age: number }) {
  console.log(name);
  console.log(age);
}
```

---

## ✅ Call

```ts id="f2"
printUser({ name: "Fahim", age: 25 });
```

---

## 🟢 Output

```txt id="f3"
Fahim
25
```

---

# 🔹 8. Array in Function Parameter

```ts id="f4"
function printFriends([a, b]: string[]) {
  console.log(a);
  console.log(b);
}
```

---

## ✅ Call

```ts id="f5"
printFriends(["Karim", "Rahim"]);
```

---

## 🟢 Output

```txt id="f6"
Karim
Rahim
```

---

# 🔥 Full Real Example

```ts id="full"
const user = {
  id: 1,
  name: {
    firstName: "Fahim",
    middleName: "Hai",
  },
  age: 25,
};

const {
  name: { firstName, middleName },
  age,
} = user;

console.log(firstName);
console.log(middleName);
console.log(age);

const friends = ["Karim", "Rahim", "Mahim"];

const [, , best] = friends;

console.log(best);
```

---

## 🟢 Output

```txt id="out"
Fahim
Hai
25
Mahim
```

---

# 🔥 Quick Summary

* Object → key দিয়ে data বের করা
* Array → position দিয়ে data বের করা
* Rename → নতুন variable name দেওয়া
* Nested → ভিতরের object access
* Skip → comma দিয়ে ignore করা

---

# 🧠 One-line memory

👉 “structure ভেঙে value বের করাই destructuring”

---

চাও হলে আমি next এ তোমাকে **Spread vs Rest vs Destructuring একসাথে interview tricky questions** দিয়ে practice করাতে পারি 👍
