# Week 4: Problem Set

## Instructions
1. Open the article link.
2. Read the problem statement carefully.
3. Click on "Try It Yourself".
4. Solve the problem on your own first.
5. Spend at least 20-25 minutes genuinely thinking and trying to optimise your approach before looking at solutions.
6. After attempting properly, go through the:
    * Better approach
    * Optimal approach given in the article.
7. Even if your solution works, try to:
    * Reduce time complexity
    * Reduce space complexity
    * Improve readability
    * Write cleaner logic
8. Dry run your code on sample test cases.
9. Try edge cases:
    * Empty arrays
    * Single element arrays
    * Negative numbers
    * Large inputs
10. Avoid directly memorizing code. Focus on:
    * Why the approach works
    * How the optimisation was derived
11. Write time and space complexity after every solution.
12. If stuck, do not instantly search for code:
    * Drawing examples
    * Observing patterns
    * Simplifying the problem
    * Solving smaller cases manually

Good Luck!

***

> **NOTE** - All the Selected problems are the most commonly occurring and highly logical problems. Spend a sufficient amount of time on them to build a strong foundation in each topic. Then, if further time permits, you can go through additional problems from resources.

---

## Linked List
*Focus: Pointer manipulation, multi-pointer strategies, structural adjustments, and sublist operations.*

### Easy
1. **Reverse a Linked List** *Requires:* Iterative tracking of previous, current, and next pointers to reverse link directions in-place without extra memory.  
   https://leetcode.com/problems/reverse-linked-list/
2. **Middle of the Linked List** *Requires:* Utilizing a two-pointer technique (slow and fast) to locate the midpoint node in a single traversal pass.  
   https://leetcode.com/problems/middle-of-the-linked-list/

### Medium
3. **Linked List Cycle II** *Requires:* Detecting a loop using Floyd’s Cycle-Finding Algorithm and mathematically finding the absolute starting node of the cycle.  
   https://leetcode.com/problems/linked-list-cycle-ii/
4. **Remove Nth Node From End of List** *Requires:* Maintaining a specific look-ahead spacing gap between two pointers to isolate and delete target nodes cleanly.  
   https://leetcode.com/problems/remove-nth-node-from-end-of-list/
5. **Reorder List** *Requires:* Combining core link steps: extracting the middle, reversing the latter partition, and weaving nodes sequentially.  
   https://leetcode.com/problems/reorder-list/

### Hard
6. **Reverse Nodes in k-Group** *Requires:* Modifying linked nodes in distinct structural blocks while keeping cross-group segment pointers unbroken.  
   https://leetcode.com/problems/reverse-nodes-in-k-group/
7. **Merge k Sorted Lists** *Requires:* Combining collection structures cleanly using empirical divide-and-conquer logic or an optimized custom heap structure.  
   https://leetcode.com/problems/merge-k-sorted-lists/

---

## Stack / Queue
*Focus: Linear order restrictions, resource parsing algorithms, tracking structural metrics, and monotonic behaviors.*

### Easy
1. **Valid Parentheses** *Requires:* Pushing open tokens onto an operational stack and checking strict symmetry constraints when popping.  
   https://leetcode.com/problems/valid-parentheses/
2. **Implement Queue using Stacks** *Requires:* Emulating standard FIFO access mechanics using two distinct LIFO storage components efficiently.  
   https://leetcode.com/problems/implement-queue-using-stacks/

### Medium
3. **Min Stack** *Requires:* Preserving access to structural minimum values alongside the primary element stack within continuous O(1) boundaries.  
   https://leetcode.com/problems/min-stack/
4. **Daily Temperatures** *Requires:* Standardizing an index-based monotonic stack approach to dynamically seek the nearest greater element ahead.  
   https://leetcode.com/problems/daily-temperatures/
5. **Design Circular Queue** *Requires:* Engineering a fixed spatial data ring buffer optimizing memory write boundaries using wrap-around modulo paths.  
   https://leetcode.com/problems/design-circular-queue/

### Hard
6. **Largest Rectangle in Histogram** *Requires:* Deploying a monotonic layout to calculate the bounding left and right minimal value steps for any single node profile.  
   https://leetcode.com/problems/largest-rectangle-in-histogram/
7. **Trapping Rain Water** *Requires:* Accumulating localized surface elevations using an tracking boundary stack or contrasting array pointer configurations.  
   https://leetcode.com/problems/trapping-rain-water/

---

## Deque
*Focus: Double-ended element processing, sliding boundaries, and persistent range calculations.*

### Medium
1. **Design Circular Deque** *Requires:* Coding explicit pointer links to accommodate continuous entry and removal operations smoothly from both terminals.  
   https://leetcode.com/problems/design-circular-deque/
2. **Longest Continuous Subarray With Absolute Diff Less Than Or Equal To Limit** *Requires:* Balancing dual monotonic deques concurrently to track real-time extreme ranges across flexible dynamic sliding spans.  
   https://leetcode.com/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit/

### Hard
3. **Sliding Window Maximum** *Requires:* Processing array ranges by enforcing strict decreasing value structures within an active double-ended queue.  
   https://leetcode.com/problems/sliding-window-maximum/
4. **Max Value of Equation** *Requires:* Converting relational functions mathematically to isolate structural bounds using single-pass index optimization steps.  
   https://leetcode.com/problems/max-value-of-equation/

---

## Backtracking
*Focus: Permutation paths, constraint trees, deep state manipulation, and early recursion branch cutting.*

### Medium
1. **Subsets** *Requires:* Exploring choice trees systematically by deciding either to preserve or reject individual items at every junction.  
   https://leetcode.com/problems/subsets/
2. **Permutations** *Requires:* Creating every distinct sequence arrangement variation through explicit node swaps or boolean state tables.  
   https://leetcode.com/problems/permutations/
3. **Combination Sum** *Requires:* Navigating multi-choice recursive states where single elements can be selected infinitely until limits are passed.  
   https://leetcode.com/problems/combination-sum/
4. **Word Search** *Requires:* Executing a matrix grid search across multiple directions while tracking currently utilized cells to avoid trace loops.  
   https://leetcode.com/problems/word-search/

### Hard
5. **N-Queens** *Requires:* Placing components onto a dynamic matrix grid while handling structural vector constraints across rows, columns, and diagonals.  
   https://leetcode.com/problems/n-queens/
6. **Sudoku Solver** *Requires:* Performing a depth-first empty cell scan to fill matrix fields while reverting incorrect guesses via state backtracking.  
   https://leetcode.com/problems/sudoku-solver/
7. **Palindrome Partitioning** *Requires:* Splitting string segments systematically and using predictive checks to drop non-symmetric branch expansions immediately.  
   https://leetcode.com/problems/palindrome-partitioning/
