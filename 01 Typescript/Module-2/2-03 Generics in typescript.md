নিচে একই টিউটোরিয়ালটা আরও পরিষ্কার করে দিলাম, সাথে **output আলাদা করে সহজভাবে দেখানো আছে** যাতে তুমি দ্রুত বুঝতে পারো।

---

# 🔷 Generics in TypeScript (সহজ টিউটোরিয়াল + Output)

---

## 🔥 1. Generics আসলে কী?

👉 Generics মানে হলো এমন কোড লেখা যেটা **একাধিক টাইপ (number, string, boolean)** এর জন্য কাজ করে।

---

# 🔷 2. Without Generic (সমস্যা)

```ts
function identity(value: number): number {
  return value;
}

console.log(identity(10));
```

---

## 📤 Output

```
10
```

---

### ❌ সমস্যা কী?

👉 শুধু `number` কাজ করে
👉 string বা অন্য টাইপ দিলে কাজ করবে না

---

# 🔷 3. With Generic (Solution)

```ts
function identity<T>(value: T): T {
  return value;
}
```

👉 এখানে `T` = placeholder type
👉 পরে তুমি টাইপ ঠিক করবে

---

## 🔥 ব্যবহার

```ts
console.log(identity<number>(10));
console.log(identity<string>("Hello"));
```

---

## 📤 Output

```
10
Hello
```

---

# 🔷 4. Generic Array Example

```ts
function getArray<T>(items: T[]): T[] {
  return items;
}

const numbers = getArray<number>([1, 2, 3]);
const strings = getArray<string>(["a", "b", "c"]);

console.log(numbers);
console.log(strings);
```

---

## 📤 Output

```
[ 1, 2, 3 ]
[ 'a', 'b', 'c' ]
```

---

# 🔷 5. Generic Object Example

```ts
type User<T> = {
  id: number;
  data: T;
};

const user1: User<string> = {
  id: 1,
  data: "Admin",
};

const user2: User<number> = {
  id: 2,
  data: 100,
};

console.log(user1);
console.log(user2);
```

---

## 📤 Output

```
{ id: 1, data: 'Admin' }
{ id: 2, data: 100 }
```

---

# 🔷 6. Multiple Generic (X, Y)

```ts
function pair<X, Y>(a: X, b: Y): [X, Y] {
  return [a, b];
}

console.log(pair<number, string>(10, "Ten"));
console.log(pair<string, boolean>("OK", true));
```

---

## 📤 Output

```
[ 10, 'Ten' ]
[ 'OK', true ]
```

---

# 🔥 একদম সহজ সারাংশ

👉 `T` = যেকোনো টাইপ ধরে রাখে
👉 `<T>` = generic declare করা
👉 পরে ব্যবহার করার সময় টাইপ ঠিক করা হয়

---

# 🧠 মনে রাখার ট্রিক

👉 Generic = “same function, different types”
👉 TypeScript আগেই error ধরতে পারে (type safety)

---

# 🚀 Final বুঝা

Generics তোমাকে দেয়:

✔ Reusable code
✔ Flexible design
✔ Strong type safety

---

চাও তাহলে আমি পরের ধাপে তোমাকে **Generic Constraints (extends keyword) + real interview questions** একদম সহজ করে দেখাতে পারি 👍

