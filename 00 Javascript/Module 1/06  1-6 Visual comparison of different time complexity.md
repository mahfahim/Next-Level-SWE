<img width="1163" height="978" alt="image" src="https://github.com/user-attachments/assets/d603809e-350a-401f-b3dd-587b0a872332" />
<img width="1298" height="1041" alt="image" src="https://github.com/user-attachments/assets/2e90a18e-aa5f-47af-bab2-dc4965330c2b" />

নিচে সব main time complexity গুলো একসাথে খুব clear ভাবে code + intuition দিয়ে দেখাচ্ছি, যাতে তুমি visual difference ধরতে পারো।

---

# ১. O(1) → Constant Time

👉 input যতই বড় হোক, time same

```cpp
int getFirst(vector<int>& arr) {
    return arr[0];
}
```

## Intuition:

* শুধু ১টা operation
* data size matter করে না

👉 instant access

---

# ২. O(log n) → Logarithmic Time

👉 প্রতিবার problem half হয়ে যায়

## Example: Binary Search

```cpp
int binarySearch(vector<int>& arr, int target) {
    int l = 0, r = arr.size() - 1;

    while (l <= r) {
        int mid = (l + r) / 2;

        if (arr[mid] == target) return mid;
        else if (arr[mid] < target) l = mid + 1;
        else r = mid - 1;
    }
    return -1;
}
```

## Intuition:

* প্রতিবার half data বাদ
* 1M → ~20 steps

👉 খুব fast

---

# ৩. O(n) → Linear Time

👉 সব element একবার করে check

```cpp
bool search(vector<int>& arr, int target) {
    for (int x : arr) {
        if (x == target) return true;
    }
    return false;
}
```

## Intuition:

* line ধরে হাঁটা
* worst case সব দেখবে

---

# ৪. O(n log n) → Efficient Sorting

👉 fast sorting algorithms

## Example: Merge Sort

```cpp
void mergeSort(vector<int>& arr) {
    // conceptually:
    // divide → sort → merge
}
```

(Full code long, তাই concept important)

## Intuition:

* array divide হয় (log n)
* প্রতিবার merge হয় (n)

👉 best practical sorting complexity

---

# ৫. O(n²) → Quadratic Time

👉 nested loop

```cpp
void printPairs(vector<int>& arr) {
    for (int i = 0; i < arr.size(); i++) {
        for (int j = 0; j < arr.size(); j++) {
            cout << arr[i] << " " << arr[j] << endl;
        }
    }
}
```

## Intuition:

* প্রতিটা element আবার সব element এর সাথে compare

👉 slow for large data

---

# ৬. O(n³) → Cubic Time

👉 3 nested loops

```cpp
void tripleLoop(vector<int>& arr) {
    for (int i = 0; i < arr.size(); i++) {
        for (int j = 0; j < arr.size(); j++) {
            for (int k = 0; k < arr.size(); k++) {
                cout << i << j << k << endl;
            }
        }
    }
}
```

👉 extremely slow

---

# ৭. O(2ⁿ) → Exponential Time

👉 recursion explosion

```cpp
int fib(int n) {
    if (n <= 1) return n;
    return fib(n-1) + fib(n-2);
}
```

## Intuition:

* প্রতিটা call 2টা call তৈরি করে
* খুব দ্রুত explode করে

---

# ৮. O(n!) → Factorial Time

👉 permutations problem

```cpp
void permute(string s, int l, int r) {
    if (l == r) {
        cout << s << endl;
        return;
    }
    for (int i = l; i <= r; i++) {
        swap(s[l], s[i]);
        permute(s, l + 1, r);
        swap(s[l], s[i]);
    }
}
```

## Intuition:

* সব possible arrangement generate করে

👉 extremely slow (small n only)

---

# FINAL VISUAL COMPARISON

| Complexity | Speed        | Real Feeling        |
| ---------- | ------------ | ------------------- |
| O(1)       | fastest      | instant             |
| O(log n)   | very fast    | half-half cut       |
| O(n)       | normal       | line scan           |
| O(n log n) | efficient    | smart sorting       |
| O(n²)      | slow         | nested loop         |
| O(n³)      | very slow    | heavy nesting       |
| O(2ⁿ)      | extreme slow | recursion explosion |
| O(n!)      | worst        | permutations        |

---

# Final intuition

👉 Big-O মানে হলো:
“input বাড়লে algorithm কতটা slow হবে”

---

চাও হলে আমি এগুলোর একটা **graphical growth chart (visual curve comparison)** বানিয়ে বুঝিয়ে দিতে পারি, সেটা interview preparation এর জন্য খুব helpful।
