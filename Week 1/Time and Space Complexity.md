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
```python
def get_first(arr):
    return arr[0]
```
**How we get O(1):**
There is exactly 1 operation here — reading a value at a fixed index. It doesn't matter if `arr` has 10 or 10 million elements; the step count never changes. Any fixed number of operations, no matter how large, simplifies to O(1).

---

### 2. Logarithmic Time — O(log n)
The problem is **halved** at each step, so the algorithm barely slows down even for enormous inputs. Even for n = 1,000,000,000, log₂(n) ≈ 30 steps.

**Example — Binary Search:**
```python
def binary_search(arr, target):
    left, right = 0, len(arr) - 1
    while left <= right:
        mid = (left + right) // 2
        if arr[mid] == target:   return mid
        elif arr[mid] < target:  left = mid + 1
        else:                    right = mid - 1
    return -1
```
**How we get O(log n):**
Each iteration of the `while` loop cuts the search range in half. Starting with n elements:
- After 1st iteration → n/2 elements remain
- After 2nd iteration → n/4 elements remain
- After k iterations → n/2ᵏ elements remain

The loop stops when n/2ᵏ = 1, i.e. k = log₂(n). So the loop runs **log₂(n) times** → **O(log n)**.

---

### 3. Linear Time — O(n)
Steps grow proportionally with input size. Double the input → double the time. You're visiting each element a fixed number of times.

**Example — Find Maximum (Linear Scan):**
```python
def find_max(arr):
    max_val = arr[0]
    for num in arr:
        if num > max_val:
            max_val = num
    return max_val
```
**How we get O(n):**
The `for` loop runs exactly once for every element in `arr`. If `arr` has n elements, we do n iterations, each with O(1) work inside. Total = n × O(1) = **O(n)**.

---

### 4. Linearithmic Time — O(n log n)
Divide the input logarithmically, then do linear work at each level. This is the sweet spot for efficient, general-purpose sorting — far better than O(n²) and provably optimal for comparison-based sorting.

**Example — Merge Sort:**
```python
def merge_sort(arr):
    if len(arr) <= 1: return arr
    mid = len(arr) // 2
    left  = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])
    return merge(left, right)
```
**How we get O(n log n):**
Think of it in two parts:
- **Dividing:** We keep splitting the array in half until we reach single elements. This gives us **log₂(n) levels** of recursion.
- **Merging:** At each level, we merge all the pieces back together. Across one full level, we touch every element exactly once → **O(n) work per level**.

Total = log n levels × O(n) work per level = **O(n log n)**.

```
Level 0:  [8, 3, 5, 1]              ← 1 array, n=4 total work to merge
Level 1:  [8, 3]  [5, 1]           ← 2 arrays, still n=4 total work
Level 2:  [8] [3] [5] [1]          ← base case, no work needed
```

---

### 5. Quadratic Time — O(n²)
Two nested loops, each running n times → n × n operations. Performance degrades quickly as input grows.

**Example — Bubble Sort:**
```python
def bubble_sort(arr):
    n = len(arr)
    for i in range(n):              # runs n times
        for j in range(n - i - 1): # runs ~n times for each i
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
    return arr
```
**How we get O(n²):**
- Outer loop runs **n times**.
- Inner loop runs **(n-1), (n-2), ..., 1** times across all outer iterations.
- Total comparisons = (n-1) + (n-2) + ... + 1 = **n(n-1)/2**
- Dropping the constant and lower-order term: **O(n²)**.

> ⚠️ n = 10,000 → ~50 million comparisons. Avoid for large inputs.

---

### 6. Exponential & Factorial — O(2ⁿ), O(n!)

Work **doubles** with every new element (O(2ⁿ)) or grows catastrophically fast (O(n!)). These appear in brute-force solutions and problems that enumerate all possibilities. Dynamic Programming is often used to bring O(2ⁿ) problems down to O(n) or O(n²).

**Example — Naive Fibonacci (O(2ⁿ)):**
```python
def fibonacci(n):
    if n <= 1: return n
    return fibonacci(n - 1) + fibonacci(n - 2)
```
**How we get O(2ⁿ):**
Each call spawns **2 more calls**. Visualising the recursion tree for `fib(4)`:
```
                fib(4)
              /        \
          fib(3)       fib(2)
          /    \       /    \
       fib(2) fib(1) fib(1) fib(0)
       /    \
    fib(1) fib(0)
```
At each level the number of nodes doubles. With n levels, the total nodes ≈ **2ⁿ** → **O(2ⁿ)**.

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

The key takeaway from the graph: O(1) and O(log n) stay nearly flat even as n grows large. O(n) and O(n log n) rise steadily. O(n²) curves sharply upward. O(2ⁿ) shoots off the chart almost immediately — which is why exponential algorithms are unusable for any meaningful input size.

---

## **Space Complexity**

Measures the **memory** an algorithm uses relative to input size.

> **Auxiliary Space** = extra memory used *excluding* the input. This is what interviews usually mean by "space complexity."

**Fixed Part** — doesn't grow with input (simple variables, constants)

**Variable Part** — grows with input (dynamic arrays, recursion call stack)

```python
# O(1) auxiliary space — only a couple of variables, no matter the input size
def sum_array(arr):
    total = 0
    for num in arr:
        total += num
    return total

# O(n) auxiliary space — creates a new array proportional to input
def copy_array(arr):
    return [num for num in arr]

# O(n) auxiliary space — each recursive call adds a frame to the call stack
def factorial(n):
    if n == 0: return 1
    return n * factorial(n - 1)
```

> 📦 `factorial(4)` call stack at peak: `f(4) → f(3) → f(2) → f(1) → f(0)` — 5 frames → O(n) space.

---

## **Key Tips**

- `arr.sort()` is **O(n log n)**, not O(1)
- `x in list` is **O(n)**; `x in set` is **O(1)**
- A loop where `i *= 2` or `i //= 2` runs in **O(log n)**, not O(n)
- Recursive functions use **O(depth)** stack space even with no extra arrays

---

## **Resources**

- [GeeksforGeeks — Time & Space Complexity](https://www.geeksforgeeks.org/time-complexity-and-space-complexity/)
- [W3Schools DSA Complexity](https://www.w3schools.com/dsa/dsa_timecomplexity_theory.php)
- [Big-O Cheat Sheet](https://www.bigocheatsheet.com/)
- [Visualgo — Algorithm Visualizations](https://visualgo.net/)
