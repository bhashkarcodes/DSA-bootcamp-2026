# Divide and Conquer

Home > Week 3 > Divide and Conquer

Week 3 · Topic 1 of 3 · Prerequisites: Recursion, Arrays, Sorting basics

## Why This Topic Now

You know recursion. Divide and Conquer is the first major *strategy* that puts recursion to work on real problems. Every algorithm here — binary search, merge sort, quicksort — shows up constantly in interviews and in the problems you will tackle from Week 4 onwards.

## The Pattern

Every divide-and-conquer algorithm follows three steps:

| Step | What happens |
|------|-------------|
| **Divide** | Split the problem into smaller independent subproblems |
| **Conquer** | Solve each subproblem (recursively, until a base case) |
| **Merge** | Combine the subproblem results into the final answer |

## Complexity: The Master Theorem

For algorithms that split a problem of size n into `a` subproblems of size `n/b`, each with `f(n)` work outside the recursion:

```
T(n) = a·T(n/b) + f(n)
```

You will see this applied concretely for each algorithm below.

---

## Binary Search

Operates on a **sorted** array. Repeatedly halves the search space to find a target in O(log n) time.

**How it works:**
1. Set `low = 0`, `high = n - 1`
2. Compute `mid = low + (high - low) / 2`
3. If `arr[mid] == target` → found, return `mid`
4. If `target > arr[mid]` → search right half: `low = mid + 1`
5. If `target < arr[mid]` → search left half: `high = mid - 1`
6. If `low > high` → not found, return `-1`

**Complexity:** T(n) = T(n/2) + O(1) → **O(log n)**

![Binary Search](https://github.com/bhashkarcodes/DSA-bootcamp-2026/raw/main/Week-3/images/Binary_search.png)

**C++**
```cpp
int binarySearch(vector<int>& arr, int x) {
    int low = 0, high = arr.size() - 1;
    while (low <= high) {
        int mid = low + (high - low) / 2;
        if (arr[mid] == x) return mid;
        if (arr[mid] < x)  low = mid + 1;
        else               high = mid - 1;
    }
    return -1;
}
```

**Java**
```java
static int binarySearch(int[] arr, int x) {
    int low = 0, high = arr.length - 1;
    while (low <= high) {
        int mid = low + (high - low) / 2;
        if (arr[mid] == x) return mid;
        if (arr[mid] < x)  low = mid + 1;
        else               high = mid - 1;
    }
    return -1;
}
```

**Python**
```python
def binary_search(arr, x):
    low, high = 0, len(arr) - 1
    while low <= high:
        mid = low + (high - low) // 2   # use // for integer division
        if arr[mid] == x:   return mid
        elif arr[mid] < x:  low = mid + 1
        else:               high = mid - 1
    return -1
```

> **Common bug:** Python's `/` is float division. Use `//` for the midpoint, or you will get a `TypeError` when indexing.

---

## Merge Sort
Merge Sort works by recursively dividing the input array into two halves, recursively sorting the two halves and finally merging them back together to obtain the sorted array.

- Divide the list or array recursively into two halves until it can no more be divided.
- Each subarray is sorted individually using the merge sort algorithm.
- The sorted subarrays are merged back together in sorted order. The process continues until all elements from both subarrays have been merged.

- Guaranteed O(n log n) in all cases.

**Complexity:** T(n) = 2T(n/2) + O(n) → **O(n log n)**  
**Space:** O(n) for the temporary arrays during merge.

![Merge Sort](https://github.com/bhashkarcodes/DSA-bootcamp-2026/raw/main/Week-3/images/Merge_Sort.png)

**C++**
```cpp
void merge(vector<int>& arr, int left, int mid, int right) {
    vector<int> L(arr.begin() + left, arr.begin() + mid + 1);
    vector<int> R(arr.begin() + mid + 1, arr.begin() + right + 1);

    int i = 0, j = 0, k = left;
    while (i < L.size() && j < R.size())
        arr[k++] = (L[i] <= R[j]) ? L[i++] : R[j++];
    while (i < L.size()) arr[k++] = L[i++];
    while (j < R.size()) arr[k++] = R[j++];
}

void mergeSort(vector<int>& arr, int left, int right) {
    if (left >= right) return;
    int mid = left + (right - left) / 2;
    mergeSort(arr, left, mid);
    mergeSort(arr, mid + 1, right);
    merge(arr, left, mid, right);
}
```

**Java**
```java
static void merge(int[] arr, int l, int m, int r) {
    int n1 = m - l + 1, n2 = r - m;
    int[] L = Arrays.copyOfRange(arr, l, m + 1);
    int[] R = Arrays.copyOfRange(arr, m + 1, r + 1);

    int i = 0, j = 0, k = l;
    while (i < n1 && j < n2)
        arr[k++] = (L[i] <= R[j]) ? L[i++] : R[j++];
    while (i < n1) arr[k++] = L[i++];
    while (j < n2) arr[k++] = R[j++];
}

static void mergeSort(int[] arr, int l, int r) {
    if (l >= r) return;
    int m = l + (r - l) / 2;
    mergeSort(arr, l, m);
    mergeSort(arr, m + 1, r);
    merge(arr, l, m, r);
}
```

**Python**
```python
def merge(arr, left, mid, right):
    L = arr[left:mid + 1]
    R = arr[mid + 1:right + 1]
    i = j = 0
    k = left
    while i < len(L) and j < len(R):
        if L[i] <= R[j]:
            arr[k] = L[i]; i += 1
        else:
            arr[k] = R[j]; j += 1
        k += 1
    while i < len(L): arr[k] = L[i]; i += 1; k += 1
    while j < len(R):  arr[k] = R[j]; j += 1; k += 1

def merge_sort(arr, left, right):
    if left >= right: return
    mid = left + (right - left) // 2
    merge_sort(arr, left, mid)
    merge_sort(arr, mid + 1, right)
    merge(arr, left, mid, right)
```

---

## Quick Sort

Picks a pivot, partitions the array so everything smaller is on the left and everything larger on the right, then recurses on both sides.

There are mainly three steps in the algorithm:

- Choose a Pivot: Select an element from the array as the pivot. The choice of pivot can vary (e.g., first element, last element, random element, or median).
- Partition the Array: Re arrange the array around the pivot. After partitioning, all elements smaller than the pivot will be on its left, and all elements greater than the pivot will be on its right.(It can be done using Naive Partition, Lomuto Partition, Hoare's Partition)
- Recursively Call: Recursively apply the same process to the two partitioned sub-arrays. Base Case: The recursion stops when there is only one element left in the sub-array, as a single element is already sorted.


**Average complexity:** O(n log n) · **Worst case:** O(n²) when the pivot is always the smallest or largest element (e.g. already-sorted array with last-element pivot).  
**Space:** O(log n) average stack depth.

Below uses **Lomuto partition** with the rightmost element as pivot.

![Quick Sort](https://github.com/bhashkarcodes/DSA-bootcamp-2026/raw/main/Week-3/images/quicksort_rightmost_pivot.png)

**C++**
```cpp
int partition(vector<int>& arr, int low, int high) {
    int pivot = arr[high], i = low - 1;
    for (int j = low; j < high; j++)
        if (arr[j] < pivot) swap(arr[++i], arr[j]);
    swap(arr[i + 1], arr[high]);
    return i + 1;
}

void quickSort(vector<int>& arr, int low, int high) {
    if (low < high) {
        int pi = partition(arr, low, high);
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}
```

**Java**
```java
static int partition(int[] arr, int low, int high) {
    int pivot = arr[high], i = low - 1;
    for (int j = low; j < high; j++)
        if (arr[j] < pivot) { int t = arr[++i]; arr[i] = arr[j]; arr[j] = t; }
    int t = arr[i + 1]; arr[i + 1] = arr[high]; arr[high] = t;
    return i + 1;
}

static void quickSort(int[] arr, int low, int high) {
    if (low < high) {
        int pi = partition(arr, low, high);
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}
```

**Python**
```python
def partition(arr, low, high):
    pivot, i = arr[high], low - 1
    for j in range(low, high):
        if arr[j] < pivot:
            i += 1
            arr[i], arr[j] = arr[j], arr[i]
    arr[i + 1], arr[high] = arr[high], arr[i + 1]
    return i + 1

def quick_sort(arr, low, high):
    if low < high:
        pi = partition(arr, low, high)
        quick_sort(arr, low, pi - 1)
        quick_sort(arr, pi + 1, high)
```

---

## Matrix Multiplication

Given matrices `mat1` (n×m) and `mat2` (m×q), compute their product (n×q). Standard naive algorithm is O(n³).

**C++**
```cpp
vector<vector<int>> multiply(vector<vector<int>>& mat1, vector<vector<int>>& mat2) {
    int n = mat1.size(), m = mat1[0].size(), q = mat2[0].size();
    vector<vector<int>> res(n, vector<int>(q, 0));
    for (int i = 0; i < n; i++)
        for (int j = 0; j < q; j++)
            for (int k = 0; k < m; k++)
                res[i][j] += mat1[i][k] * mat2[k][j];
    return res;
}
```

**Java**
```java
static int[][] multiply(int[][] mat1, int[][] mat2) {
    int n = mat1.length, m = mat1[0].length, q = mat2[0].length;
    int[][] res = new int[n][q];
    for (int i = 0; i < n; i++)
        for (int j = 0; j < q; j++)
            for (int k = 0; k < m; k++)
                res[i][j] += mat1[i][k] * mat2[k][j];
    return res;
}
```

**Python**
```python
def multiply(mat1, mat2):
    n, m, q = len(mat1), len(mat1[0]), len(mat2[0])
    res = [[0] * q for _ in range(n)]
    for i in range(n):
        for j in range(q):
            for k in range(m):
                res[i][j] += mat1[i][k] * mat2[k][j]
    return res
```

---

## Advanced Topics (Reference)

You might find the upcoming 3 algos a bit intimidating but don't get disheartened, look at it multiple times and u will conquer it!!

Also people who are just starting DSA might skip it for now and come again later to conquer it!!

### Karatsuba Algorithm (Fast Multiplication)

Multiplies two n-digit numbers in O(n^1.585) instead of O(n²) by reducing 4 recursive multiplications to 3. Built on the observation that the middle cross-term can be derived from just P1, P2, P3.

[Full explanation and code — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/karatsuba-algorithm-for-fast-multiplication-using-divide-and-conquer-algorithm/)

![Karatsuba](https://github.com/bhashkarcodes/DSA-bootcamp-2026/raw/main/Week-3/images/Karatsuba_algorithm_for_fast_multiplication.png)

### Convex Hull (Divide and Conquer)

Finds the smallest convex polygon containing all given points. The D&C approach splits points by x-coordinate, finds hull for each half, then merges by finding upper and lower tangent lines.

[Full explanation and code — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/convex-hull-using-divide-and-conquer-algorithm/)

![Convex Hull](https://github.com/bhashkarcodes/DSA-bootcamp-2026/raw/main/Week-3/images/Convex_Hull.png)

### Quickhull Algorithm

A D&C approach to convex hull that works similarly to quicksort: find extremes, pick the farthest point from the connecting line, recurse on the two outer edges.

[Full explanation and code — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/quickhull-algorithm-convex-hull/)

---

## Before You Move On

- Can you implement binary search without looking at your notes?
- Do you understand why merge sort is always O(n log n) but quicksort can be O(n²)?
- Can you trace through one pass of Lomuto partition on a small array?
- When would you prefer merge sort over quicksort in practice?

## Resources

- [Divide and Conquer — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/divide-and-conquer/)

Previous: Recursion | Week 3 Overview | Next: Dynamic Programming
