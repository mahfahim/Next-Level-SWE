চল, একদম সহজভাবে **Type vs Interface** টা বুঝি—tutorial স্টাইলে, সাথে example + output 👇

---

# 🔷 1. Type কী?

👉 `type` দিয়ে আমরা custom data type বানাই

```ts
type User = {
  name: string;
  age: number;
};
```

👉 এখন এটা ব্যবহার করি:

```ts
const user1: User = {
  name: "Fahim",
  age: 22,
};

console.log(user1);
```

✔️ Output:

```
{ name: "Fahim", age: 22 }
```

---

# 🔷 2. Interface কী?

👉 `interface` ও একই কাজ করে (object shape define)

```ts
interface User2 {
  name: string;
  age: number;
}
```

👉 ব্যবহার:

```ts
const user2: User2 = {
  name: "Rahim",
  age: 25,
};

console.log(user2);
```

✔️ Output:

```
{ name: "Rahim", age: 25 }
```

---

# 🤔 তাহলে Type আর Interface আলাদা কোথায়?

চল step by step দেখি 👇

---

# 🔶 3. Extend (Inheritance)

### 👉 Interface extend করতে পারে

```ts
interface Person {
  name: string;
}

interface Student extends Person {
  roll: number;
}

const s1: Student = {
  name: "Karim",
  roll: 10,
};

console.log(s1);
```

✔️ Output:

```
{ name: "Karim", roll: 10 }
```

---

### 👉 Type দিয়েও করা যায় (intersection)

```ts
type Person2 = {
  name: string;
};

type Student2 = Person2 & {
  roll: number;
};

const s2: Student2 = {
  name: "Hasan",
  roll: 20,
};

console.log(s2);
```

✔️ Output:

```
{ name: "Hasan", roll: 20 }
```

---

# 🔶 4. Interface merge হয় (🔥 important)

👉 একই নামের interface বারবার লিখলে merge হয়

```ts
interface Animal {
  name: string;
}

interface Animal {
  age: number;
}

const a1: Animal = {
  name: "Cat",
  age: 2,
};

console.log(a1);
```

✔️ Output:

```
{ name: "Cat", age: 2 }
```

---

### ❌ Type এ এটা হয় না

```ts
type Animal2 = {
  name: string;
};

// ❌ Error দিবে
type Animal2 = {
  age: number;
};
```

---

# 🔶 5. Type বেশি powerful (🔥)

👉 Type দিয়ে union, primitive, function সব করা যায়

```ts
type ID = string | number;

const id1: ID = "123";
const id2: ID = 456;

console.log(id1, id2);
```

✔️ Output:

```
123 456
```

---

### 👉 Function type

```ts
type Add = (a: number, b: number) => number;

const add: Add = (a, b) => a + b;

console.log(add(2, 3));
```

✔️ Output:

```
5
```

---

# 🔶 6. Interface mainly object এর জন্য

```ts
interface Product {
  name: string;
  price: number;
}

const p1: Product = {
  name: "Book",
  price: 200,
};

console.log(p1);
```

✔️ Output:

```
{ name: "Book", price: 200 }
```

---

# 🔥 Final Comparison (easy way)

| বিষয়          | Type       | Interface     |
| ------------- | ---------- | ------------- |
| Object define | ✅          | ✅             |
| Extend        | ✅ (& দিয়ে) | ✅ (extends)   |
| Merge         | ❌          | ✅             |
| Union         | ✅          | ❌             |
| Function type | ✅          | ❌ (direct না) |

---

# 💡 কখন কোনটা ব্যবহার করবে?

👉 Use **interface** যখন:

* object / class structure বানাও
* future এ extend / merge লাগতে পারে

👉 Use **type** যখন:

* union দরকার (`string | number`)
* function type লাগবে
* complex type বানাতে হবে

---

# 🧠 সহজভাবে মনে রাখো

* **interface = object structure**
* **type = flexible (সবকিছু)**

---

চাও হলে আমি তোমাকে
👉 real project example (API response)
👉 interview trick question
👉 common mistake

এগুলোও বুঝিয়ে দিতে পারি 👍
