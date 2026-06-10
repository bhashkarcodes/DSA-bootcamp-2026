# **Backtracking**

> 📌 **Week 4 — Study Material**

---

## **What is Backtracking?**

Backtracking is a recursive technique where you **build a solution step by step**, and whenever you reach a point where the current path can't lead to a valid solution, you **undo the last step and try a different option**.

Think of it like navigating a maze — you go down a path, and if you hit a dead end, you back up to the last junction and try another route.

Every backtracking solution follows this skeleton:

```cpp
void backtrack(/* current state */) {
    if (/* base case / goal reached */) {
        // record the solution
        return;
    }

    for (/* each possible choice */) {
        // make the choice
        backtrack(/* updated state */);
        // undo the choice  ← this is the "backtrack" step
    }
}
```

---

## **Pattern 1 — Subsets (Pick or Skip)**

At each element, you have two choices — include it or skip it.

**Example — All Subsets of an Array:**

```cpp
void subsets(vector<int>& nums, int index, vector<int>& current, vector<vector<int>>& result) {
    result.push_back(current);              // every state is a valid subset

    for (int i = index; i < nums.size(); i++) {
        current.push_back(nums[i]);         // choose nums[i]
        subsets(nums, i + 1, current, result);  // recurse with remaining
        current.pop_back();                 // undo choice
    }
}

// Call: subsets(nums, 0, current, result)
```

**How we get O(2ⁿ) time, O(n) space:**
- Each element is either in or out → **2ⁿ subsets** total → **O(2ⁿ) time**
- Recursion depth is at most n (one element per level) → **O(n) space**

---

## **Pattern 2 — Permutations**

Generate all possible orderings of elements.

**Example — All Permutations of an Array:**

```cpp
void permutations(vector<int>& nums, int start, vector<vector<int>>& result) {
    if (start == nums.size()) {
        result.push_back(nums);    // base case: all positions filled
        return;
    }

    for (int i = start; i < nums.size(); i++) {
        swap(nums[start], nums[i]);              // choose: put nums[i] at position start
        permutations(nums, start + 1, result);   // recurse on remaining positions
        swap(nums[start], nums[i]);              // undo choice
    }
}

// Call: permutations(nums, 0, result)
```

**How we get O(n! × n) time, O(n) space:**
- There are **n!** permutations, and copying each one takes O(n) → **O(n × n!) time**
- Recursion depth is n (one position filled per level) → **O(n) space**

---

## **Pattern 3 — Combination Sum**

Find all combinations that satisfy a condition. A key addition here is **pruning** — cutting off branches early when they can't possibly lead to a solution.

**Example — Combination Sum (elements can be reused, target sum):**

```cpp
void combinationSum(vector<int>& candidates, int target, int index,
                    vector<int>& current, vector<vector<int>>& result) {
    if (target == 0) {
        result.push_back(current);   // found a valid combination
        return;
    }

    for (int i = index; i < candidates.size(); i++) {
        if (candidates[i] > target) break;   // pruning: no point going further

        current.push_back(candidates[i]);
        combinationSum(candidates, target - candidates[i], i, current, result);
        current.pop_back();                  // undo choice
    }
}

// Call with sorted candidates: combinationSum(candidates, target, 0, current, result)
```

**How pruning helps:**
Without the `break`, we'd explore every branch. Sorting `candidates` first means once `candidates[i] > target`, all further elements are also too large — so we cut the entire remaining branch. This drastically reduces the number of recursive calls in practice.

---

## **Pattern 4 — Constraint Satisfaction (N-Queens)**

Place elements under strict constraints. At each step, check if the current placement is valid before recursing.

**Example — N-Queens (place N queens on N×N board, none attacking each other):**

```cpp
bool isSafe(vector<string>& board, int row, int col, int n) {
    // check column above
    for (int i = 0; i < row; i++)
        if (board[i][col] == 'Q') return false;

    // check upper-left diagonal
    for (int i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--)
        if (board[i][j] == 'Q') return false;

    // check upper-right diagonal
    for (int i = row - 1, j = col + 1; i >= 0 && j < n; i--, j++)
        if (board[i][j] == 'Q') return false;

    return true;
}

void nQueens(vector<string>& board, int row, int n, vector<vector<string>>& result) {
    if (row == n) {
        result.push_back(board);    // all queens placed
        return;
    }

    for (int col = 0; col < n; col++) {
        if (isSafe(board, row, col, n)) {
            board[row][col] = 'Q';              // place queen
            nQueens(board, row + 1, n, result); // recurse to next row
            board[row][col] = '.';              // undo placement
        }
    }
}

// Call: vector<string> board(n, string(n, '.')); nQueens(board, 0, n, result);
```

**How we get O(n!) time, O(n) space:**
- In the first row, n choices. In the second, at most n-1 valid choices, and so on → **O(n!) time** in the worst case
- Recursion depth = n (one row per level) → **O(n) space** (ignoring the board itself)

---

## **The Three Steps — Always**

Every backtracking problem follows the same three steps inside the loop:

```
1. Make a choice
2. Recurse
3. Undo the choice
```

If you find yourself writing backtracking code and step 3 is missing, something is wrong.

---

## **Backtracking vs Brute Force**

| | Brute Force | Backtracking |
|---|-------------|--------------|
| Approach | Generate all possibilities, then filter | Prune invalid paths early, never generate them |
| Efficiency | Explores everything | Skips entire subtrees that can't work |
| When to use | Only when no constraints exist | When constraints allow early elimination |

---

## **Key Tips**

- Always **sort the input** when order doesn't matter and pruning is possible (like Combination Sum)
- The `undo` step must **exactly mirror** the `make choice` step — if you `push_back`, you `pop_back`; if you `swap`, you `swap` again
- Use a **visited array** or **boolean flags** for permutation problems to avoid reusing elements
- Backtracking is essentially **DFS on a decision tree** — every node is a partial solution, every leaf is either valid or pruned
- Time complexity is hard to pin down exactly — it depends on how much pruning occurs. O(2ⁿ) and O(n!) are worst-case bounds

---

## **Resources**

- [GeeksforGeeks — Backtracking](https://www.geeksforgeeks.org/backtracking-algorithms/)
- [LeetCode — Backtracking Problems](https://leetcode.com/tag/backtracking/)
- [Striver — Recursion & Backtracking Playlist](https://takeuforward.org/strivers-a2z-dsa-course/strivers-a2z-dsa-course-sheet-2/)
