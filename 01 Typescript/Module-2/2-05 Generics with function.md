চলো, এবার আমরা **Generic with Function** নিয়ে বিস্তারিতভাবে আলোচনা করি, যাতে তুমি খুব সহজে বুঝতে পারো। শুরু থেকে শেষ পর্যন্ত টিউটোরিয়াল স্টাইলে।

---

# 🧠 **Generic with Function (Full Detailed Tutorial)**

## 🔥 1. **Generic Function কী?**

Generic function হলো এমন একটি function, যা বিভিন্ন ধরনের data type এর সাথে কাজ করতে পারে।
তুমি একবার function লিখবে, কিন্তু সেটা যেকোনো টাইপের data দিয়ে কাজ করবে।

---

# 🔷 2. **Function Without Generic (Normal Function)**

ধরি, তুমি একটা function বানালে যা দুটি number যোগ করবে:

```ts id="v7ptdb"
const addNumbers = (num1: number, num2: number) => num1 + num2;
```

এখানে:

* function শুধু `number` টাইপের data নিয়েই কাজ করছে।
* তুমি যদি `string` দিয়ে যোগ করতে চাও, তাহলে error পাবে।

---

## 👉 Example with String (without Generic)

```ts id="b93k7b"
const addStrings = (str1: string, str2: string) => str1 + str2;
```

এখন তুমি `number` দিয়ে কল করতে পারবে না।

---

# 🔥 3. **Generic Function দিয়ে কি হবে?**

Generic function তোমাকে flexibility দেবে।
একই function দিয়ে তুমি `number`, `string`, বা যেকোনো type এর data ব্যবহার করতে পারবে।

```ts id="bzs94s"
const add = <T>(num1: T, num2: T): T => {
  return (num1 as any) + (num2 as any);
};
```

এখানে:

* `<T>` → এটা **type placeholder**, যা তুমি পরে যেকোনো টাইপ দিয়ে replace করতে পারবে।
* `num1: T, num2: T` → function দুইটি parameter নেবে, যেগুলোর টাইপ হবে `T` (যেকোনো টাইপ হতে পারে)।
* `T` = return টাইপও `T` হতে হবে, মানে function result ঐ same টাইপের হবে।

---

# 🔷 4. **Function Call with Generic**

## 👉 Example 1: **Number Addition**

```ts id="t39y6e"
const sum = add<number>(2, 3);
console.log(sum);  // Output: 5
```

এখানে:

* `T` → `number` দিয়ে replace হয়েছে।
* `add<number>` মানে তুমি function-এ number টাইপ দিচ্ছো।

### ✔ Output:

```ts id="56p8q5"
5
```

---

## 👉 Example 2: **String Concatenation**

```ts id="y2ub75"
const result = add<string>("Hello, ", "World!");
console.log(result);  // Output: "Hello, World!"
```

এখানে:

* `T` → `string` দিয়ে replace হয়েছে।
* `add<string>` মানে তুমি function-এ string টাইপ দিচ্ছো।

### ✔ Output:

```ts id="t98q4v"
"Hello, World!"
```

---

# 🔷 5. **Why Generic Functions are Useful?**

## 💡 **Key Advantages:**

1. **Code Reusability**:
   তুমি একই function দিয়ে বিভিন্ন টাইপের data নিয়ে কাজ করতে পারো।
   (যেমন, number, string, object, ইত্যাদি)

2. **Type Safety**:
   TypeScript automatically type check করে দেয়, যাতে ভুল টাইপের data পাস না হয়। যেমন:

   ```ts id="w63k4j"
   add<number>("Hello", 2);  // Error: Argument of type 'string' is not assignable to parameter of type 'number'.
   ```

3. **Flexibility**:
   তুমি একই function বিভিন্ন context এ ব্যবহার করতে পারো।
   (যেমন, add function numbers বা strings বা object এর সাথে)

---

# 🔥 6. **Example with More Complex Types (e.g., Arrays, Objects)**

## 👉 Example 3: **Array Concatenation**

ধরি, তুমি arrays যোগ করতে চাও:

```ts id="49k1mj"
const concatArray = <T>(arr1: T[], arr2: T[]): T[] => {
  return [...arr1, ...arr2];
};
```

এখন তুমি function কে array দিয়ে call করতে পারো।

```ts id="w2i7fg"
const mergedArray = concatArray<number>([1, 2], [3, 4]);
console.log(mergedArray);  // Output: [1, 2, 3, 4]
```

### ✔ Output:

```ts id="k4b9cl"
[1, 2, 3, 4]
```

---

## 👉 Example 4: **Object Merging**

ধরি, তুমি objects merge করতে চাও:

```ts id="5vphyn"
const mergeObjects = <T>(obj1: T, obj2: T): T => {
  return { ...obj1, ...obj2 };
};
```

এখন তুমি function কে objects দিয়ে call করতে পারো:

```ts id="f7e8nv"
const obj1 = { name: "Alice", age: 25 };
const obj2 = { city: "Dhaka" };
const mergedObject = mergeObjects(obj1, obj2);
console.log(mergedObject);  // Output: { name: "Alice", age: 25, city: "Dhaka" }
```

### ✔ Output:

```ts id="pgrm9u"
{ name: "Alice", age: 25, city: "Dhaka" }
```

---

# 🧾 7. **Key Concepts to Remember**

| Concept      | Explanation                                                         |
| ------------ | ------------------------------------------------------------------- |
| `<T>`        | Placeholder for any type                                            |
| `T[]`        | Array of type `T`                                                   |
| `T[]` vs `T` | `T[]` = Array, `T` = Single value                                   |
| `as any`     | Convert to any type if necessary (unsafe, but useful in some cases) |

---

# 🚀 8. **Final Summary**

* **Generic Function**: তোমার function অনেক ধরনের data type-এ কাজ করতে পারে।
* **Type Safety**: TypeScript automatically সঠিক টাইপ check করবে।
* **Reusability**: একবার লিখে একাধিকবার ব্যবহার করতে পারবে।

Generic function তোমাকে flexibility দেয়, যাতে তুমি কোড কম রাখতে পারো, আবার যে কোনো টাইপের সাথে কাজ করতে পারো।

---

চাও হলে আমি পরবর্তী **Generic with Constraints** বা **Real Interview Questions** নিয়ে বিস্তারিত আলোচনা করতে পারি!

