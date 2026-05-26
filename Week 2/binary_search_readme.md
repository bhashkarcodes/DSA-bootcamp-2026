
---

## What is Binary Search?

Binary Search is a searching algorithm that operates on a **sorted or monotonic search space**, repeatedly dividing it into halves to find a target value in **logarithmic time O(log N)**.

Think of it like finding a word in a dictionary — you don't start from page 1. You open the middle, decide whether to go left or right, and repeat. That intuition is exactly what binary search formalizes.

---

## The Algorithm

The core idea: at every step, **eliminate half the search space**.

1. Set `low = 0`, `high = n - 1`.
2. Find `mid = low + (high - low) / 2`.
   > use this form — not `(low + high) / 2`. It avoids **integer overflow** in Java/C++.
3. If `arr[mid] == target` → return `mid`.
4. If `target < arr[mid]` → answer is in the left half → `high = mid - 1`.
5. If `target > arr[mid]` → answer is in the right half → `low = mid + 1`.
6. Repeat until `low > high`. If not found, return `-1`.

**Dry Run Example:**

Array: `[2, 5, 8, 12, 16, 23, 38, 56, 72, 91]` | Target: `23`

| Step | low | high | mid | arr[mid] | Action |
|------|-----|------|-----|----------|--------|
| 1 | 0 | 9 | 4 | 16 | 23 > 16 → go right |
| 2 | 5 | 9 | 7 | 56 | 23 < 56 → go left |
| 3 | 5 | 6 | 5 | 23 |  Found at index 5 |

---

## Time & Space Complexity

This is where binary search is better than linear search. Let's understand *why* it's O(log N).

Every step cuts the search space **in half**. Starting with N elements:

```
After 1 step  →  N/2 remain
After 2 steps →  N/4 remain
After k steps →  N/2ᵏ remain
```

We stop when `N/2ᵏ = 1` → `k = log₂(N)`.

So in the worst case, we only need **log₂(N) comparisons**. 
**Space Complexity:**
- Iterative → **O(1)** (just a few variables)
- Recursive → **O(log N)** (log N frames on the call stack)

---

## Implementation: Two Ways to Write Binary Search

The algorithm is identical — only the style differs. **Iterative** (O(1) space, preferred in practice) uses a while loop. **Recursive** (O(log N) space due to call stack) calls itself on a smaller half — cleaner but risks stack overflow on huge inputs.

---

### 1. Iterative Binary Search

**Python**
```python
def binary_search(arr, x):
    low, high = 0, len(arr) - 1
    while low <= high:
        mid = low + (high - low) // 2
        if arr[mid] == x:    return mid
        elif arr[mid] < x:   low = mid + 1
        else:                high = mid - 1
    return -1
```

**Java**
```java
static int binarySearch(int[] arr, int x) {
    int low = 0, high = arr.length - 1;
    while (low <= high) {
        int mid = low + (high - low) / 2;
        if (arr[mid] == x)      return mid;
        else if (arr[mid] < x)  low = mid + 1;
        else                    high = mid - 1;
    }
    return -1;
}
```

**C++**
```cpp
int binarySearch(vector<int>& arr, int x) {
    int low = 0, high = arr.size() - 1;
    while (low <= high) {
        int mid = low + (high - low) / 2;
        if (arr[mid] == x)      return mid;
        else if (arr[mid] < x)  low = mid + 1;
        else                    high = mid - 1;
    }
    return -1;
}
```

---

### 2. Recursive Binary Search

**Python**
```python
def binary_search(arr, low, high, x):
    if high >= low:
        mid = low + (high - low) // 2
        if arr[mid] == x:    return mid
        elif arr[mid] > x:   return binary_search(arr, low, mid - 1, x)
        else:                return binary_search(arr, mid + 1, high, x)
    return -1
```

**Java**
```java
static int binarySearch(int[] arr, int low, int high, int x) {
    if (high >= low) {
        int mid = low + (high - low) / 2;
        if (arr[mid] == x)      return mid;
        if (arr[mid] > x)       return binarySearch(arr, low, mid - 1, x);
        return binarySearch(arr, mid + 1, high, x);
    }
    return -1;
}
```

**C++**
```cpp
int binarySearch(vector<int>& arr, int low, int high, int x) {
    if (high >= low) {
        int mid = low + (high - low) / 2;
        if (arr[mid] == x)      return mid;
        if (arr[mid] > x)       return binarySearch(arr, low, mid - 1, x);
        return binarySearch(arr, mid + 1, high, x);
    }
    return -1;
}
```

> **Base case:** `high < low` means the search space is empty.

---

## Variations of Binary Search

### Variation 1 — Lower Bound

Lower bound finds the **first position where a value ≥ target exists** — the leftmost index where the target could be inserted while keeping the array sorted.

The condition shifts slightly from standard binary search: when `arr[mid] >= target`, the current `mid` is recorded as a potential answer and the search continues **left** (`high = mid - 1`) to check for an earlier valid position. When `arr[mid] < target`, the search moves right as usual.

**Use cases:** Finding the insertion point for a target, finding the first element ≥ some value, range query boundaries.

> In C++, `std::lower_bound()` does this out of the box.

---

### Variation 2 — Upper Bound

Upper bound finds the **first position where a value > target exists** — the rightmost index where the target could be inserted while keeping the array sorted.

When `arr[mid] <= target`, the search moves right. When `arr[mid] > target`, `mid` is recorded and the search continues **left** (`high = mid - 1`). A useful property: `upper_bound(target) - lower_bound(target)` gives the **count of occurrences** of the target in O(log N).

**Use cases:** Finding the end boundary of a range, counting occurrences, finding the first element strictly greater than a value.

> In C++, `std::upper_bound()` handles this. In Java, it is implemented manually.


---

### Variation 3 — First and Last Occurrence

These target a specific value and find its exact first or last position in an array with duplicates — unlike lower/upper bound which operate on inequality conditions.

**First occurrence:** When `arr[mid] == target`, the index is recorded and the search continues **left** (`high = mid - 1`) to check for an earlier duplicate.

**Last occurrence:** When `arr[mid] == target`, the index is recorded and the search continues **right** (`low = mid + 1`) to check for a later duplicate.

It is worth noting that the first occurrence of a target equals `lower_bound(target)` only when the target is confirmed to exist in the array. The two are related but serve different purposes.

**Use cases:** Finding the range `[first, last]` of a repeated element, 

---

### Variation 4 — Binary Search on Answer

This variation does not search an array directly. Instead, it searches an abstract **answer space** for the optimal value that satisfies a given condition.

**The pattern:**
- Define a range `[low, high]` of possible answers.
- Write a `isValid(mid)` predicate that returns true/false.
- Since valid and invalid answers are monotonic — all valid on one side, all invalid on the other — binary search finds the exact boundary efficiently.



---


## Resources: 
- [Binary Search](https://www.geeksforgeeks.org/dsa/binary-search/)

- [Variants of Binary Search](https://www.geeksforgeeks.org/dsa/variants-of-binary-search/)

- [Binary Search Questions](https://www.geeksforgeeks.org/dsa/most-asked-binary-search-interview-questions/)

- [Binary Search on Answers](https://www.geeksforgeeks.org/dsa/binary-search-on-answer-tutorial-with-problems/)

---

