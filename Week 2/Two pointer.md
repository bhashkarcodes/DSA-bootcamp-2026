# **Two Pointers**

> 📌 **Week 1 — Study Material**

---

## **What is the Two Pointer Technique?**

Two pointers is a pattern where you use **two index variables** that move through a data structure — usually an array or string — to solve a problem efficiently. Instead of using nested loops to check all pairs (O(n²)), two pointers lets you do it in a single pass (O(n)).

The two pointers typically either:
- **Start from opposite ends** and move toward each other
- **Start from the same end** and move at different speeds (slow & fast pointer)

---

## **Pattern 1 — Opposite Ends**

Both pointers start at the two ends of the array and move inward until they meet.

**Example — Two Sum (Sorted Array):**
Given a sorted array, find two numbers that add up to a target.

```cpp
vector<int> twoSumSorted(vector<int>& arr, int target) {
    int left = 0, right = arr.size() - 1;

    while (left < right) {
        int currentSum = arr[left] + arr[right];
        if (currentSum == target)
            return {left, right};
        else if (currentSum < target)
            left++;     // need a bigger number, move left pointer right
        else
            right--;    // need a smaller number, move right pointer left
    }

    return {};
}
```

**How we get O(n):**
In the worst case, the two pointers together traverse the entire array once — `left` moves right and `right` moves left, and they never cross. Each element is visited at most once → **O(n)** time, **O(1)** space.

---

## **Pattern 2 — Same Direction (Slow & Fast)**

Both pointers start at the same end but move at different speeds. The fast pointer explores ahead while the slow pointer marks a position.

**Example — Remove Duplicates from Sorted Array:**
Remove duplicates in-place and return the length of the unique part.

```cpp
int removeDuplicates(vector<int>& arr) {
    if (arr.empty()) return 0;

    int slow = 0;  // marks the last unique element

    for (int fast = 1; fast < arr.size(); fast++) {
        if (arr[fast] != arr[slow]) {  // found a new unique element
            slow++;
            arr[slow] = arr[fast];     // place it after the last unique
        }
    }

    return slow + 1;
}
```

**How we get O(n):**
`fast` moves from index 1 to n-1 — exactly one pass through the array. `slow` only moves when a new unique element is found, never exceeding `fast`. Total steps = n → **O(n)** time, **O(1)** space (modified in-place, no extra array).

---

## **Pattern 3 — Sliding Window (Variable Size)**

A variation where the two pointers define a **window** that expands and shrinks based on a condition.

**Example — Longest Subarray with Sum ≤ k:**

```cpp
int longestSubarray(vector<int>& arr, int k) {
    int left = 0, currentSum = 0, maxLen = 0;

    for (int right = 0; right < arr.size(); right++) {
        currentSum += arr[right];         // expand window

        while (currentSum > k) {          // shrink window if condition violated
            currentSum -= arr[left];
            left++;
        }

        maxLen = max(maxLen, right - left + 1);
    }

    return maxLen;
}
```

**How we get O(n):**
`right` moves forward n times total. `left` also only moves forward — it never goes back. So across the entire run, `left` moves at most n times too. Total pointer movements ≤ 2n → **O(n)** time, **O(1)** space.

---

## **When to Use Two Pointers**

| Signal in the problem | Pattern to reach for |
|-----------------------|----------------------|
| Sorted array + find a pair | Opposite ends |
| Remove/count elements in-place | Slow & fast |
| Longest/shortest subarray with condition | Sliding window |
| Palindrome check | Opposite ends |
| Cycle detection in linked list | Slow & fast (Floyd's) |

---

## **Key Tips**

- Two pointers almost always requires the array to be **sorted**, or the problem to have a structure that lets you decide which pointer to move
- Always check the **loop condition** carefully — `left < right` vs `left <= right` matters
- The technique trades the second loop for pointer logic → drops O(n²) to **O(n)**
- Space is almost always **O(1)** since you're just maintaining two index variables

---

## **Resources**

- [GeeksforGeeks — Two Pointer Technique](https://www.geeksforgeeks.org/two-pointers-technique/)
- [LeetCode — Two Pointers Problems](https://leetcode.com/tag/two-pointers/)
- [Visualgo — Array Sorting (to understand sorted array assumption)](https://visualgo.net/en/sorting)
