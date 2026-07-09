# Monotonic Stack

A way of using a regular stack with one extra rule — the values inside it are always kept sorted (either increasing or decreasing) — to answer "what's the next bigger/smaller thing?" questions in `O(n)` instead of `O(n²)`. If you've ever written a nested loop to find, for every element, the next element to its right that's bigger or smaller, this is the topic that fixes that.

---

## Quick Recap: What Is a Stack?

A stack is a simple data structure that only lets you do two things: **push** (add an element to the top) and **pop** (remove the element from the top). The last element you pushed is always the first one you pop — this is called **LIFO** (Last In, First Out), like a stack of plates where you can only take from the top.

```
push 1 → push 2 → push 3        pop → removes 3        pop → removes 2
   [1]      [1,2]     [1,2,3]      →    [1,2]              →    [1]
```

A monotonic stack is just a regular stack with one extra rule layered on top, explained next.


## 1. What Is a Monotonic Stack?

A monotonic stack is a stack where, at every point in time, the elements are either strictly increasing or strictly decreasing from bottom to top. You maintain this order yourself: before pushing a new element, you pop off everything at the top that would break the order.

```
Monotonic decreasing stack (top is always the smallest seen so far that's still "active"):

push 5  →  [5]
push 3  →  [5, 3]
push 8  →  pop 3, pop 5 (both smaller than 8)  →  [8]
push 2  →  [8, 2]
```

The popping is the whole point. When you pop an element because a new, bigger (or smaller) one came along, that new element **is the answer** for whatever you popped — that's how you get next-greater, next-smaller, and similar results, all in one pass.

---

## 2. Why Use One? (Recognizing the Pattern)

Ask yourself:
- Does the problem ask for the **next** or **previous** greater/smaller element, for every position in an array?
- Does it involve **comparing each element against its neighbors** to find a boundary — like "how far can this bar extend before something taller blocks it"?
- Would the brute-force solution be a nested loop, where the inner loop re-scans elements you've already looked at?

If yes, a monotonic stack is probably the fix. The key insight: once a smaller element is "blocked" by a bigger one next to it, that smaller element becomes useless for every future comparison — so you can throw it away (pop it) for good, and never re-check it.

---

## 3. The Core Template

Both directions (looking for "next" or "previous") and both kinds (increasing or decreasing) use the same skeleton: walk through the array once, and before pushing the current element, pop anything on the stack that the current element "beats."

**Decreasing stack → finds the next *greater* element**

**Python**
```python
def next_greater_elements(arr):
    n = len(arr)
    result = [-1] * n
    stack = []  # stores indices

    for i in range(n):
        while stack and arr[stack[-1]] < arr[i]:
            popped_index = stack.pop()
            result[popped_index] = arr[i]
        stack.append(i)

    return result
```

**Java**
```java
public int[] nextGreaterElements(int[] arr) {
    int n = arr.length;
    int[] result = new int[n];
    Arrays.fill(result, -1);
    Deque<Integer> stack = new ArrayDeque<>();

    for (int i = 0; i < n; i++) {
        while (!stack.isEmpty() && arr[stack.peek()] < arr[i]) {
            int poppedIndex = stack.pop();
            result[poppedIndex] = arr[i];
        }
        stack.push(i);
    }
    return result;
}
```

**C++**
```cpp
vector<int> nextGreaterElements(vector<int>& arr) {
    int n = arr.size();
    vector<int> result(n, -1);
    stack<int> st;

    for (int i = 0; i < n; i++) {
        while (!st.empty() && arr[st.top()] < arr[i]) {
            int poppedIndex = st.top();
            st.pop();
            result[poppedIndex] = arr[i];
        }
        st.push(i);
    }
    return result;
}
```

Notice the result is filled in **while popping**, not while pushing — that's the part beginners trip on most.

---

## 4. Why It's `O(n)` and Not `O(n²)`

Every element gets pushed onto the stack **exactly once**, and popped off **at most once**. So across the entire run, the total number of pushes and pops combined can never exceed `2n`. Even though there's a `while` loop sitting inside a `for` loop, the work doesn't multiply — it adds up. That's what makes it `O(n)` instead of `O(n²)`.

---

## 5. The Four Variants You'll Actually See

Every monotonic stack problem boils down to one of these four questions. The only things that change are: which direction you scan, and whether the stack is increasing or decreasing.

| You want... | Stack type | Scan direction | Answer recorded... |
|---|---|---|---|
| Next Greater Element | Decreasing | Left → Right | when popping |
| Next Smaller Element | Increasing | Left → Right | when popping |
| Previous Greater Element | Decreasing | Left → Right | from what's left on top, after pushing |
| Previous Smaller Element | Increasing | Left → Right | from what's left on top, after pushing |

A simple way to keep this straight: for "next" problems, you record the answer **as you pop** something off (the new element is what made it pop, so the new element is the answer). For "previous" problems, you record the answer **after pushing** the current element — whatever's now sitting just below it on the stack is its nearest qualifying neighbor.

---

## 6. The Different Types of Monotonic Stack Problems

### Type 1 — Next Greater / Next Smaller Element

The foundational pattern — covered already in the Core Template above. Every other type below is a variation of this same idea, applied to a slightly different question.

---

### Type 2 — Previous Greater / Previous Smaller Element

Instead of looking forward, you want the nearest qualifying element **behind** the current one. You still scan left to right, but instead of waiting to pop, you read the answer straight off the stack right after you finish popping and just before (or after) pushing the current element.

**Example: Previous Smaller Element** — for every index, find the closest element to its left that's smaller than it.

**Python**
```python
def previous_smaller_elements(arr):
    n = len(arr)
    result = [-1] * n
    stack = []  # increasing stack of values

    for i in range(n):
        while stack and stack[-1] >= arr[i]:
            stack.pop()
        result[i] = stack[-1] if stack else -1
        stack.append(arr[i])

    return result
```

**Java**
```java
public int[] previousSmallerElements(int[] arr) {
    int n = arr.length;
    int[] result = new int[n];
    Deque<Integer> stack = new ArrayDeque<>();

    for (int i = 0; i < n; i++) {
        while (!stack.isEmpty() && stack.peek() >= arr[i]) {
            stack.pop();
        }
        result[i] = stack.isEmpty() ? -1 : stack.peek();
        stack.push(arr[i]);
    }
    return result;
}
```

**C++**
```cpp
vector<int> previousSmallerElements(vector<int>& arr) {
    int n = arr.size();
    vector<int> result(n, -1);
    stack<int> st;

    for (int i = 0; i < n; i++) {
        while (!st.empty() && st.top() >= arr[i]) {
            st.pop();
        }
        result[i] = st.empty() ? -1 : st.top();
        st.push(arr[i]);
    }
    return result;
}
```

This single building block — previous smaller (or greater) on one pass, next smaller (or greater) on another — is what powers histogram and subarray-range problems further down.

---

### Type 3 — Span / Width Problems (Index Distance, Not Value)

Sometimes you don't want the *value* of the next/previous qualifying element — you want **how far away** it is. The stack stores indices instead of values, and the answer is computed as a distance (`i - poppedIndex`) at the moment of popping.

**Example: Stock Span Problem** — for each day's stock price, find how many consecutive days (including today) the price has been less than or equal to today's price.

**Python**
```python
def stock_span(prices):
    n = len(prices)
    span = [0] * n
    stack = []  # stores indices, decreasing by price

    for i in range(n):
        while stack and prices[stack[-1]] <= prices[i]:
            stack.pop()
        span[i] = i + 1 if not stack else i - stack[-1]
        stack.append(i)

    return span
```

**Java**
```java
public int[] stockSpan(int[] prices) {
    int n = prices.length;
    int[] span = new int[n];
    Deque<Integer> stack = new ArrayDeque<>();

    for (int i = 0; i < n; i++) {
        while (!stack.isEmpty() && prices[stack.peek()] <= prices[i]) {
            stack.pop();
        }
        span[i] = stack.isEmpty() ? i + 1 : i - stack.peek();
        stack.push(i);
    }
    return span;
}
```

**C++**
```cpp
vector<int> stockSpan(vector<int>& prices) {
    int n = prices.size();
    vector<int> span(n);
    stack<int> st;

    for (int i = 0; i < n; i++) {
        while (!st.empty() && prices[st.top()] <= prices[i]) {
            st.pop();
        }
        span[i] = st.empty() ? i + 1 : i - st.top();
        st.push(i);
    }
    return span;
}
```

The same "distance instead of value" trick is what solves Daily Temperatures and similar problems.

---

### Type 4 — Boundary / Area Problems (Histogram-Style)

Here, the monotonic stack finds, for every element, the widest range where that element is the **minimum** (or maximum). Once you know how far left and right an element can "reach" before something smaller blocks it, you can compute an area, a sum, or a contribution for that element — and do this for every element in one linear pass.

**Example: Largest Rectangle in Histogram** — given bar heights, find the largest rectangular area that fits under the skyline.

For each bar, its rectangle can extend left and right until a shorter bar blocks it. An increasing stack of indices finds exactly that boundary: when a shorter bar shows up, every taller bar sitting on the stack has just found its right boundary, and whatever's left under it on the stack is its left boundary.

**Python**
```python
def largest_rectangle_area(heights):
    stack = []  # increasing stack of indices
    max_area = 0
    heights = heights + [0]  # sentinel to flush the stack at the end

    for i in range(len(heights)):
        while stack and heights[stack[-1]] >= heights[i]:
            height = heights[stack.pop()]
            width = i if not stack else i - stack[-1] - 1
            max_area = max(max_area, height * width)
        stack.append(i)

    return max_area
```

**Java**
```java
public int largestRectangleArea(int[] heights) {
    int n = heights.length;
    int[] extended = new int[n + 1];
    System.arraycopy(heights, 0, extended, 0, n); // sentinel 0 at the end

    Deque<Integer> stack = new ArrayDeque<>();
    int maxArea = 0;

    for (int i = 0; i <= n; i++) {
        while (!stack.isEmpty() && extended[stack.peek()] >= extended[i]) {
            int height = extended[stack.pop()];
            int width = stack.isEmpty() ? i : i - stack.peek() - 1;
            maxArea = Math.max(maxArea, height * width);
        }
        stack.push(i);
    }
    return maxArea;
}
```

**C++**
```cpp
int largestRectangleArea(vector<int>& heights) {
    heights.push_back(0); // sentinel to flush the stack at the end
    stack<int> st;
    int maxArea = 0;

    for (int i = 0; i < heights.size(); i++) {
        while (!st.empty() && heights[st.top()] >= heights[i]) {
            int height = heights[st.top()];
            st.pop();
            int width = st.empty() ? i : i - st.top() - 1;
            maxArea = max(maxArea, height * width);
        }
        st.push(i);
    }
    return maxArea;
}
```

The same "find left and right boundary, then multiply" idea — just with a sum instead of an area — is what solves Sum of Subarray Minimums and Sum of Subarray Ranges.

---

### Type 5 — Monotonic Deque (Sliding Window Maximum/Minimum)

When a problem combines a **sliding window** with a need for the **max or min inside that window**, a plain stack isn't enough, because old elements need to leave from the front, not just the top. A **deque** (double-ended queue) fixes this: it stays monotonic just like a stack, but you're also allowed to remove expired elements from the front.

**Example: Sliding Window Maximum** — given an array and window size `k`, return the maximum of every window as it slides across.

Keep a decreasing deque of indices. Before adding a new index, remove smaller elements from the back (they can never be the max while the new, bigger element is still in the window) and remove the front if it's fallen outside the window. The front of the deque is always the current window's maximum.

**Python**
```python
from collections import deque

def max_sliding_window(nums, k):
    dq = deque()  # stores indices, decreasing by value
    result = []

    for i in range(len(nums)):
        if dq and dq[0] == i - k:
            dq.popleft()

        while dq and nums[dq[-1]] < nums[i]:
            dq.pop()

        dq.append(i)

        if i >= k - 1:
            result.append(nums[dq[0]])

    return result
```

**Java**
```java
public int[] maxSlidingWindow(int[] nums, int k) {
    int n = nums.length;
    int[] result = new int[n - k + 1];
    Deque<Integer> dq = new ArrayDeque<>(); // stores indices

    for (int i = 0; i < n; i++) {
        if (!dq.isEmpty() && dq.peekFirst() == i - k) {
            dq.pollFirst();
        }
        while (!dq.isEmpty() && nums[dq.peekLast()] < nums[i]) {
            dq.pollLast();
        }
        dq.offerLast(i);

        if (i >= k - 1) {
            result[i - k + 1] = nums[dq.peekFirst()];
        }
    }
    return result;
}
```

**C++**
```cpp
vector<int> maxSlidingWindow(vector<int>& nums, int k) {
    deque<int> dq; // stores indices
    vector<int> result;

    for (int i = 0; i < nums.size(); i++) {
        if (!dq.empty() && dq.front() == i - k) {
            dq.pop_front();
        }
        while (!dq.empty() && nums[dq.back()] < nums[i]) {
            dq.pop_back();
        }
        dq.push_back(i);

        if (i >= k - 1) {
            result.push_back(nums[dq.front()]);
        }
    }
    return result;
}
```

This is the same monotonic-stack invariant as before — each element enters and leaves at most once — just with a front-removal rule layered on top for the window boundary.

---

## 7. More Problems to Practice

Once the five types above feel natural, these are good problems to test yourself on. No solutions here on purpose — work through which type each one belongs to, and build it from the template.

- [Next Greater Element I](https://leetcode.com/problems/next-greater-element-i/)
- [Next Greater Element II](https://leetcode.com/problems/next-greater-element-ii/)
- [Daily Temperatures](https://leetcode.com/problems/daily-temperatures/)
- [Online Stock Span](https://leetcode.com/problems/online-stock-span/)
- [Asteroid Collision](https://leetcode.com/problems/asteroid-collision/)
- [Sum of Subarray Minimums](https://leetcode.com/problems/sum-of-subarray-minimums/)
- [Sum of Subarray Ranges](https://leetcode.com/problems/sum-of-subarray-ranges/)
- [Remove K Digits](https://leetcode.com/problems/remove-k-digits/)
- [Min Stack](https://leetcode.com/problems/min-stack/)
- [Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)
- [Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)
- [Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/)
- [Maximal Rectangle](https://leetcode.com/problems/maximal-rectangle/)

---

## 8. Useful External Resources

- [Monotonic Stack — Hello Interview](https://www.hellointerview.com/learn/code/stack/monotonic-stack) — clear walkthrough of the next-greater-element template with diagrams.
- [Next Greater Element in Array — GeeksforGeeks](https://www.geeksforgeeks.org/dsa/next-greater-element/) — brute force vs. monotonic stack comparison with code.
- [A Comprehensive Guide and Template for Monotonic Stack Based Problems — LeetCode Discuss](https://leetcode.com/discuss/post/2347639/a-comprehensive-guide-and-template-for-m-irii/) — a single flexible template covering all four next/previous greater/smaller variants.
- [Monotonic Stack Code Template — labuladong](https://labuladong.online/en/algo/data-structure/monotonic-stack/) — explains the intuition behind the template using a simple visual analogy, plus circular array handling.

---
