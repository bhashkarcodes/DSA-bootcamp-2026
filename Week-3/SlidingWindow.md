# Sliding Window Technique

## What is a Sliding Window?

The **Sliding Window Technique** is an optimization method used to solve problems involving:

- Arrays
- Strings
- Subarrays
- Substrings
- Continuous ranges of elements

Instead of repeatedly recalculating information for overlapping ranges, we maintain a **window** and update it efficiently as it moves.

---

## Definition

A sliding window problem involves moving a fixed-size or variable-size window through a data structure (usually an array or string) to process continuous subsets efficiently.

### Key Idea

Instead of:

- Recomputing everything for every subarray/substring

We:

- Reuse information from the previous window
- Update the answer incrementally

This often reduces complexity from:

- **O(n²)** → **O(n)**
- **O(nk)** → **O(n)**

---

# Why Sliding Window?

Suppose we need information about many contiguous ranges.

A naive solution repeatedly traverses the same elements.

Sliding Window avoids this repetition by:

1. Maintaining a current window.
2. Removing elements that leave the window.
3. Adding elements that enter the window.

This allows each element to be processed only a few times.

---

# Example Problem

## Maximum Sum Subarray of Size K

### Problem Statement

Given:

- Array `arr[]`
- Integer `k`

Find the maximum sum among all subarrays of size exactly `k`.

### Example 1

```text
Input:
arr = [5, 2, -1, 0, 3]
k = 3

Output:
6

Explanation:
Subarray [5, 2, -1] has sum = 6
```

### Example 2

```text
Input:
arr = [1, 4, 2, 10, 23, 3, 1, 0, 20]
k = 4

Output:
39

Explanation:
Subarray [4, 2, 10, 23]
Sum = 39
```

---

# Naive Approach

## Idea

For every starting index:

- Compute the sum of the next `k` elements.
- Update the maximum sum.

### Complexity

```text
Time  : O(n × k)
Space : O(1)
```

### Code

```cpp
int maxSum(vector<int>& arr, int k) {
    int n = arr.size();
    int max_sum = INT_MIN;

    for (int i = 0; i <= n - k; i++) {

        int current_sum = 0;

        for (int j = 0; j < k; j++) {
            current_sum += arr[i + j];
        }

        max_sum = max(max_sum, current_sum);
    }

    return max_sum;
}
```

---

# Sliding Window Approach

## Observation

Consider:

```text
[5, 2, -1] -> sum = 6
```

Next window:

```text
[2, -1, 0]
```

Instead of computing again:

```text
2 + (-1) + 0
```

Use previous sum:

```text
new_sum = old_sum - outgoing + incoming
```

```text
new_sum = 6 - 5 + 0 = 1
```

---

## Window Movement

### Initial Window

```text
[5, 2, -1]  0  3

Window Sum = 6
Maximum Sum = 6
```

---

### Slide by 1

Remove:

```text
5
```

Add:

```text
0
```

```text
New Sum = 6 - 5 + 0
        = 1
```

Window:

```text
5 [2, -1, 0] 3
```

Maximum remains:

```text
6
```

---

### Slide Again

Remove:

```text
2
```

Add:

```text
3
```

```text
New Sum = 1 - 2 + 3
        = 2
```

Window:

```text
5 2 [-1, 0, 3]
```

Maximum remains:

```text
6
```

---

## Algorithm

### Step 1

Compute sum of first `k` elements.

```cpp
window_sum = arr[0] + arr[1] + ... + arr[k-1]
```

---

### Step 2

Store it as current answer.

```cpp
max_sum = window_sum
```

---

### Step 3

Slide the window.

For every new position:

```cpp
window_sum += incoming_element
window_sum -= outgoing_element
```

Update:

```cpp
max_sum = max(max_sum, window_sum)
```

---

## Code

```cpp
int maxSum(vector<int>& arr, int k) {

    int n = arr.size();

    if (n < k)
        return -1;

    int window_sum = 0;

    // First window
    for (int i = 0; i < k; i++) {
        window_sum += arr[i];
    }

    int max_sum = window_sum;

    // Slide window
    for (int i = k; i < n; i++) {

        window_sum += arr[i];
        window_sum -= arr[i - k];

        max_sum = max(max_sum, window_sum);
    }

    return max_sum;
}
```

---

## Complexity Analysis

### Time Complexity

```text
O(n)
```

Each element enters and leaves the window at most once.

### Space Complexity

```text
O(1)
```

Only a few variables are used.

---

# Types of Sliding Window Problems

There are two major categories.

---

## 1. Fixed Size Sliding Window

The window size remains constant.

### Pattern

```cpp
Window size = K
```

Examples:

- Maximum sum subarray of size K
- First negative number in every window of size K
- Maximum element in every window of size K
- Average of every subarray of size K

### General Steps

1. Determine window size `K`.
2. Compute answer for first window.
3. Slide by one position.
4. Remove outgoing element.
5. Add incoming element.
6. Update answer.

---

## 2. Variable Size Sliding Window

The window size changes dynamically.

### Pattern

Use two pointers:

```cpp
left
right
```

Expand:

```cpp
right++
```

Shrink:

```cpp
left++
```

depending on whether the condition is satisfied.

### Examples

- Longest substring without repeating characters
- Smallest subarray with sum ≥ K
- Longest subarray with sum ≤ K
- Fruits into baskets
- Minimum window substring

### General Steps

1. Expand the right pointer.
2. Check whether condition is satisfied.
3. If not, shrink from the left.
4. Continue until the end.

---

# How to Identify Sliding Window Problems

A problem is often a sliding window problem if:

### 1. It involves contiguous elements

Keywords:

- Subarray
- Substring
- Continuous segment
- Window

---

### 2. You need maximum/minimum values

Examples:

- Maximum sum
- Minimum length
- Longest substring
- Shortest subarray

---

### 3. Window size is given

Example:

```text
Find maximum sum subarray of size K
```

---

### 4. Naive solution uses nested loops

Typical complexity:

```text
O(n²)
```

or

```text
O(n × k)
```

Sliding window often reduces it to:

```text
O(n)
```

---

### 5. Constraints are large

Examples:

```text
n ≤ 10^5
n ≤ 10^6
```

Such constraints usually require:

```text
O(n)
```

or

```text
O(n log n)
```

solutions.

---
