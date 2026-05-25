# **Time and Space Complexity**

> 📌 **Week 1 — Study Material**

---

## **Why Does Complexity Matter?**

Imagine two programs that both find a number in a list of 1,000,000 elements. One checks every element one by one; the other uses a smarter search. The first takes 1,000,000 steps; the second, only about 20. Both are correct, but one is vastly superior at scale.

**Complexity analysis** lets us predict this *before* running the code.

---

## **Asymptotic Notation**

Describes how an algorithm's resource usage *grows* as input size `n` increases. We drop constants and lower-order terms — at scale, they don't matter.

> `5n² + 3n + 100` → **O(n²)**

| Notation | Type | Meaning |
|----------|------|---------|
| O (Big O) | Upper Bound | Worst-case scenario |
| Ω (Omega) | Lower Bound | Best-case scenario |
| Θ (Theta) | Tight Bound | Both bounds are the same function |

**Big O is the most commonly used in practice** — it tells us the maximum time an algorithm can take.

---

## **Time Complexity — Types**

### 1. Constant Time — O(1)
The algorithm takes the same number of steps regardless of input size. No loops, no recursion — just a fixed number of operations.

**Example — Array Index Access:**
```cpp
int getFirst(vector<int>& arr) {
    return arr[0];   // always 1 step, no matter how big arr is
}
```

**How we get O(1):**
There is exactly 1 operation here — reading a value at a fixed index. It doesn't matter if `arr` has 10 or 10 million elements; the step count never changes.

---

### 2. Logarithmic Time — O(log n)
The problem is **halved** at each step, so the algorithm barely slows down even for enormous inputs. Even for n = 1,000,000,000, log₂(n) ≈ 30 steps.

**Example — Binary Search:**
```cpp
int binarySearch(vector<int>& arr, int target) {
    int left = 0, right = arr.size() - 1;

    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (arr[mid] == target)      return mid;
        else if (arr[mid] < target)  left = mid + 1;   // discard left half
        else                         right = mid - 1;  // discard right half
    }
    return -1;
}
```

**How we get O(log n):**
Each iteration cuts the search range in half. Starting with n elements:
- After 1st iteration → n/2 elements remain
- After 2nd iteration → n/4 elements remain
- After k iterations → n/2ᵏ elements remain

The loop stops when n/2ᵏ = 1, i.e. k = log₂(n) → **O(log n)**.

---

### 3. Linear Time — O(n)
Steps grow proportionally with input size. Double the input → double the time.

**Example — Find Maximum (Linear Scan):**
```cpp
int findMax(vector<int>& arr) {
    int maxVal = arr[0];
    for (int num : arr) {       // visits every element exactly once
        if (num > maxVal)
            maxVal = num;
    }
    return maxVal;
}
```

**How we get O(n):**
The loop runs exactly once for every element in `arr`. n iterations × O(1) work each = **O(n)**.

---

### 4. Linearithmic Time — O(n log n)
Divide the input logarithmically, then do linear work at each level. This is the sweet spot for efficient, general-purpose sorting.

**Example — Merge Sort:**
```cpp
void merge(vector<int>& arr, int left, int mid, int right) {
    vector<int> temp;
    int i = left, j = mid + 1;
    while (i <= mid && j <= right)
        temp.push_back(arr[i] <= arr[j] ? arr[i++] : arr[j++]);
    while (i <= mid)  temp.push_back(arr[i++]);
    while (j <= right) temp.push_back(arr[j++]);
    for (int k = left; k <= right; k++)
        arr[k] = temp[k - left];
}

void mergeSort(vector<int>& arr, int left, int right) {
    if (left >= right) return;
    int mid = left + (right - left) / 2;
    mergeSort(arr, left, mid);       // log n levels of division
    mergeSort(arr, mid + 1, right);
    merge(arr, left, mid, right);    // O(n) merge work at each level
}
```

**How we get O(n log n):**
- **Dividing:** We keep splitting in half → **log₂(n) levels** of recursion
- **Merging:** At each level, we touch every element once → **O(n) work per level**

Total = log n levels × O(n) work = **O(n log n)**.

```
Level 0:  [8, 3, 5, 1]          ← n=4 total work to merge
Level 1:  [8, 3]  [5, 1]        ← still n=4 total work
Level 2:  [8] [3] [5] [1]       ← base case, no work needed
```

---

### 5. Quadratic Time — O(n²)
Two nested loops, each running n times → n × n operations.

**Example — Bubble Sort:**
```cpp
void bubbleSort(vector<int>& arr) {
    int n = arr.size();
    for (int i = 0; i < n; i++) {               // runs n times
        for (int j = 0; j < n - i - 1; j++) {  // runs ~n times for each i
            if (arr[j] > arr[j + 1])
                swap(arr[j], arr[j + 1]);
        }
    }
}
```

**How we get O(n²):**
- Outer loop runs **n times**
- Inner loop runs **(n-1) + (n-2) + ... + 1 = n(n-1)/2** times total
- Dropping the constant → **O(n²)**

> ⚠️ n = 10,000 → ~50 million comparisons. Avoid for large inputs.

---

### 6. Exponential & Factorial — O(2ⁿ), O(n!)

Work **doubles** with every new element (O(2ⁿ)) or grows catastrophically fast (O(n!)). These appear in brute-force solutions. Dynamic Programming is often used to bring O(2ⁿ) problems down to O(n) or O(n²).

**Example — Naive Fibonacci (O(2ⁿ)):**
```cpp
int fibonacci(int n) {
    if (n <= 1) return n;
    return fibonacci(n - 1) + fibonacci(n - 2);  // two recursive calls each time
}
```

**How we get O(2ⁿ):**
Each call spawns 2 more calls. At each level the number of nodes doubles:
```
             fib(4)
           /        \
       fib(3)       fib(2)
       /    \       /    \
    fib(2) fib(1) fib(1) fib(0)
    /    \
 fib(1) fib(0)
```
With n levels, total nodes ≈ **2ⁿ** → **O(2ⁿ)**.

> n = 50 → over a quadrillion operations with naive recursion.

---

### Complexity Growth Comparison

`O(1) < O(log n) < O(n) < O(n log n) < O(n²) < O(n³) < O(2ⁿ) < O(n!)`

| n | O(1) | O(log n) | O(n) | O(n log n) | O(n²) | O(2ⁿ) |
|---|------|----------|------|------------|-------|--------|
| 1 | 1 | 0 | 1 | 0 | 1 | 2 |
| 10 | 1 | 3 | 10 | 33 | 100 | 1,024 |
| 100 | 1 | 7 | 100 | 664 | 10,000 | 💀 |
| 1,000 | 1 | 10 | 1,000 | 9,966 | 1,000,000 | 💀 |

O(1) and O(log n) stay nearly flat even as n grows large. O(n) and O(n log n) rise steadily. O(n²) curves sharply upward. O(2ⁿ) shoots off the chart almost immediately — which is why exponential algorithms are unusable for any meaningful input size.

---

## **Space Complexity**

Measures the **memory** an algorithm uses relative to input size.

> **Auxiliary Space** = extra memory used *excluding* the input. This is what interviews usually mean by "space complexity."

**Fixed Part** — doesn't grow with input (simple variables, constants)

**Variable Part** — grows with input (dynamic arrays, recursion call stack)

```cpp
// O(1) auxiliary space — only a couple of variables, no matter the input size
int sumArray(vector<int>& arr) {
    int total = 0;
    for (int num : arr)
        total += num;
    return total;
}

// O(n) auxiliary space — creates a new array proportional to input
vector<int> copyArray(vector<int>& arr) {
    vector<int> result(arr.begin(), arr.end());
    return result;
}

// O(n) auxiliary space — each recursive call adds a frame to the call stack
int factorial(int n) {
    if (n == 0) return 1;
    return n * factorial(n - 1);
}
```

> 📦 `factorial(4)` call stack at peak: `f(4) → f(3) → f(2) → f(1) → f(0)` — 5 frames → O(n) space.

---

## **Key Tips**

- `sort()` in C++ is **O(n log n)**, not O(1)
- Searching in a `vector` is **O(n)**; searching in an `unordered_set` is **O(1)**
- A loop where `i *= 2` or `i /= 2` runs in **O(log n)**, not O(n)
- Recursive functions use **O(depth)** stack space even with no extra arrays

---

## **Resources**

- [GeeksforGeeks — Time & Space Complexity](https://www.geeksforgeeks.org/time-complexity-and-space-complexity/)
- [W3Schools DSA Complexity](https://www.w3schools.com/dsa/dsa_timecomplexity_theory.php)
- [Big-O Cheat Sheet](https://www.bigocheatsheet.com/)
- [Visualgo — Algorithm Visualizations](https://visualgo.net/)
