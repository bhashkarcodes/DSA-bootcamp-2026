# **Dynamic Programming**

> 📌 **Week 8 — Study Material**

---

## **What is Dynamic Programming?**

Dynamic Programming (DP) is a technique for solving problems by **breaking them into overlapping subproblems**, solving each subproblem **once**, and **storing the result** to avoid recomputation.

It applies when a problem has two properties:
- **Optimal Substructure** — the optimal solution to the problem can be built from optimal solutions to its subproblems
- **Overlapping Subproblems** — the same subproblems are solved repeatedly (unlike divide & conquer where subproblems are independent)

---

## **Two Approaches**

### Top-Down (Memoization)
Write the recursion naturally, but cache results in a map or array so each subproblem is computed only once.

### Bottom-Up (Tabulation)
Solve subproblems iteratively starting from the smallest, building up to the final answer. No recursion, no stack overhead.

---

## **Pattern 1 — Linear DP**

Each state depends only on a fixed number of previous states.

**Example — Fibonacci (the DP way):**

Naive recursion recomputes `fib(3)`, `fib(2)` etc. multiple times. DP fixes this.

```cpp
// Top-Down (Memoization)
int fib(int n, vector<int>& memo) {
    if (n <= 1) return n;
    if (memo[n] != -1) return memo[n];     // already computed, return cached
    return memo[n] = fib(n - 1, memo) + fib(n - 2, memo);
}
// Call: vector<int> memo(n + 1, -1); fib(n, memo);

// Bottom-Up (Tabulation)
int fib(int n) {
    if (n <= 1) return n;
    vector<int> dp(n + 1);
    dp[0] = 0; dp[1] = 1;
    for (int i = 2; i <= n; i++)
        dp[i] = dp[i - 1] + dp[i - 2];    // build from smaller subproblems
    return dp[n];
}
```

**How we get O(n) time, O(n) space:**
Each of the n subproblems is computed exactly once and stored. No repeated work → **O(n) time**. The memo/dp array stores n values → **O(n) space**. (Can be further optimized to O(1) space by keeping only the last two values.)

---

## **Pattern 2 — 0/1 Knapsack**

At each step you make a binary choice — take an item or skip it. Classic inclusion/exclusion pattern.

**Example — 0/1 Knapsack:**
Given items with weights and values, maximize value within a weight capacity.

```cpp
int knapsack(vector<int>& weights, vector<int>& values, int capacity, int n) {
    vector<vector<int>> dp(n + 1, vector<int>(capacity + 1, 0));

    for (int i = 1; i <= n; i++) {
        for (int w = 0; w <= capacity; w++) {
            // skip item i
            dp[i][w] = dp[i - 1][w];

            // take item i (only if it fits)
            if (weights[i - 1] <= w)
                dp[i][w] = max(dp[i][w], values[i - 1] + dp[i - 1][w - weights[i - 1]]);
        }
    }
    return dp[n][capacity];
}
```

**How we get O(n × W) time, O(n × W) space:**
We fill an `n × W` table, one cell at a time, each in O(1) → **O(n × W) time and space** (where W = capacity). This is called **pseudo-polynomial** time — it depends on the value of W, not just the number of items.

---

## **Pattern 3 — Longest Common Subsequence (LCS)**

Two-sequence DP — the state depends on indices into two separate sequences.

**Example — LCS of two strings:**

```cpp
int lcs(string& a, string& b) {
    int m = a.size(), n = b.size();
    vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));

    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (a[i - 1] == b[j - 1])
                dp[i][j] = 1 + dp[i - 1][j - 1];   // characters match, extend LCS
            else
                dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);  // take best of skipping either
        }
    }
    return dp[m][n];
}
```

**How we get O(m × n) time, O(m × n) space:**
We fill an `m × n` table where each cell is computed in O(1) → **O(m × n) time and space**.

**Recurrence visualised for `a = "ACE"`, `b = "ABCDE"`:**
```
    ""  A  B  C  D  E
""   0  0  0  0  0  0
A    0  1  1  1  1  1
C    0  1  1  2  2  2
E    0  1  1  2  2  3   ← answer is 3 (ACE)
```

---

## **Pattern 4 — Longest Increasing Subsequence (LIS)**

Each state depends on all previous states that satisfy a condition.

**Example — LIS:**

```cpp
int lis(vector<int>& nums) {
    int n = nums.size();
    vector<int> dp(n, 1);   // every element is an LIS of length 1 by itself

    for (int i = 1; i < n; i++) {
        for (int j = 0; j < i; j++) {
            if (nums[j] < nums[i])                    // nums[i] can extend the subsequence ending at j
                dp[i] = max(dp[i], dp[j] + 1);
        }
    }
    return *max_element(dp.begin(), dp.end());
}
```

**How we get O(n²) time, O(n) space:**
For each of the n elements, we look back at all previous elements → n + (n-1) + ... + 1 = **O(n²) time**. The dp array stores one value per element → **O(n) space**. (Can be reduced to O(n log n) using binary search.)

---

## **Memoization vs Tabulation**

| | Memoization (Top-Down) | Tabulation (Bottom-Up) |
|---|------------------------|------------------------|
| Style | Recursive + cache | Iterative table |
| Order | Solves only needed subproblems | Solves all subproblems |
| Stack space | O(depth) extra | No recursion overhead |
| Easier to write | When recursion is intuitive | When the order of subproblems is clear |

---

## **How to Identify a DP Problem**

Ask these questions:
1. Does the problem ask for an **optimal value** (max, min, count, longest, shortest)?
2. Can the answer be expressed using smaller versions of the same problem?
3. Are the same subproblems being solved repeatedly?

If yes to all three → DP is likely the right tool.

---

## **Key Tips**

- Always start by writing the **recursive solution** first, then add memoization — it's much easier than jumping straight to tabulation
- The **recurrence relation** is the heart of any DP solution — get it right first, optimizing space comes later
- **State definition** is everything — clearly define what `dp[i]` or `dp[i][j]` represents before writing any code
- Space can often be reduced from O(n²) to O(n) by observing that only the **previous row** of the table is needed at any point

---

## **Resources**

- [GeeksforGeeks — Dynamic Programming](https://www.geeksforgeeks.org/dynamic-programming/)
- [LeetCode — DP Problems](https://leetcode.com/tag/dynamic-programming/)
- [Striver — DP Series](https://takeuforward.org/dynamic-programming/striver-dp-series-dynamic-programming-problems/)
