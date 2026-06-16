# Greedy Algorithms 

---

# 1. Introduction

A **Greedy Algorithm** constructs a solution step-by-step by always making the **best possible choice at the current moment**.

Once a decision is made, it is **never reconsidered**.

> Greedy = Local optimum → Hope for global optimum

Greedy algorithms are popular because they are:

- Fast
- Memory efficient
- Easy to implement

However:

> Choosing the best immediate option does **not always guarantee** the best final answer.

---

# 2. Greedy Philosophy

Compare approaches:

### Brute Force

```text
Generate all possibilities
→ Compare all answers
→ Choose optimal
```

Complexity:
```text
Usually exponential
```

---

### Dynamic Programming

```text
Break into subproblems
→ Store answers
→ Combine
```

Complexity:
```text
Polynomial
```

---

### Greedy

```text
Choose best now
→ Move forward
→ Never revisit
```

Complexity:
```text
Often O(n log n) or O(n)
```

---

# 3. Conditions for Greedy to Work

Greedy works only if the problem satisfies:

## A. Greedy Choice Property

A globally optimal solution can be built by making the best local choice.

Example:

```text
Activity Selection
```

Choosing earliest finishing event is always safe.

---

## B. Optimal Substructure

Optimal solution contains optimal solutions to subproblems.

Example:

```text
Shortest Path
```

Shortest:

```text
A → C
```

contains shortest:

```text
A → B
```

---

# 4. Generic Greedy Framework

```cpp
while(problem_not_finished){

    choose_best_option();

    update_state();

}
```

Typical operations:

- Sort
- Pick minimum
- Pick maximum
- Use heap
- Use intervals

---

# 5. Proving Greedy Algorithms

Greedy proofs are often harder than implementation.

Main proof methods:

---

## Method 1 — Exchange Argument

Show:

> If an optimal solution doesn't make greedy choice,
> replace part of it with greedy choice
> without making solution worse.

Eventually optimal solution becomes greedy.

---

## Method 2 — Staying Ahead

Show:

After every step:

```text
Greedy ≥ Any other solution
```

Therefore final answer is optimal.

---

## Method 3 — Contradiction

Assume greedy is not optimal.

Show contradiction.

---

# 6. Coin Change Problem

## Problem

Given coin values:

```text
c1,c2,c3,...,ck
```

Each coin can be used unlimited times.

Find:

```text
Minimum number of coins
```

to make sum:

```text
n
```

---

Example:

Coins:

```text
{1,2,5,10,20,50,100,200}
```

Target:

```text
520
```

Optimal:

```text
200 + 200 + 100 + 20
```

Answer:

```text
4 coins
```

---

# Greedy Strategy

Always pick:

```text
Largest possible coin
```

until amount becomes zero.

---

## Implementation

```cpp
int minCoins(vector<int>& coins,int amount){

    sort(coins.begin(),coins.end());

    int ans=0;

    for(int i=coins.size()-1;i>=0;i--){

        if(amount>=coins[i]){

            int cnt=amount/coins[i];

            ans+=cnt;

            amount-=cnt*coins[i];
        }

        if(amount==0)
            break;
    }

    return ans;
}
```

---

## Complexity

Sorting:

```text
O(n log n)
```

Traversal:

```text
O(n)
```

Total:

```text
O(n log n)
```

---

# Why Coin Greedy Works (Euro Coins)

Observation:

Coins:

```text
1 2 5 10 20 50 100 200
```

Small coins combine naturally into larger ones.

Examples:

```text
5+5 → 10
20+20+20 → 50+10
```

Thus selecting largest coin is always safe.

---

# Counterexample (Greedy Fails)

Coins:

```text
[1,3,4]
```

Target:

```text
6
```

Greedy:

```text
4+1+1
```

Coins:

```text
3
```

Optimal:

```text
3+3
```

Coins:

```text
2
```

Thus:

> Greedy is not universally correct.

---

# 7. Activity Scheduling Problem

---

## Problem

You are given:

```text
n events
(start,end)
```

Choose maximum number of non-overlapping events.

---

Example

| Event | Start | End |
|-------|------|-----|
| A | 1 | 3 |
| B | 2 | 5 |
| C | 3 | 9 |
| D | 6 | 8 |

Maximum:

```text
2 events
```

Possible:

```text
B,D
```

---

# Wrong Greedy 1

Choose shortest duration.

Fails.

Counterexample:

```text
Long event + Long event

vs

One short event
```

Choosing short event blocks both.

---

# Wrong Greedy 2

Choose earliest start.

Fails.

Counterexample:

```text
Huge interval
```

blocks many small intervals.

---

# Correct Greedy

Choose:

```text
Event ending earliest
```

Algorithm:

1. Sort by end time
2. Pick first
3. Select next compatible event

---

## Code

```cpp
int activity(vector<pair<int,int>>& a){

    sort(a.begin(),a.end(),
        [](auto& x,auto& y){

            return x.second<y.second;

        });

    int count=1;

    int last=a[0].second;

    for(int i=1;i<a.size();i++){

        if(a[i].first>=last){

            count++;

            last=a[i].second;

        }
    }

    return count;
}
```

---

## Complexity

```text
O(n log n)
```

---

# Why Earliest Finish Works

Suppose we select an event ending later.

Then we lose possible future choices.

Selecting earliest finish preserves maximum remaining space.

Therefore:

```text
Greedy is optimal
```

---

# 8. Minimizing Sums

---

Given:

```text
a1,a2,...,an
```

Choose:

```text
x
```

to minimize:

\[
|a1-x|^c + ... +|an-x|^c
\]

---

# Case 1

\[
c=1
\]

Minimize:

\[
|a1-x|+|a2-x|+ ...
\]

Answer:

> Median

---

Example

Numbers:

```text
[1,2,9,2,6]
```

Sorted:

```text
[1,2,2,6,9]
```

Median:

```text
2
```

Minimum:

```text
12
```

---

## Rule

```text
Sum |ai−x|
```

↓

```text
Choose Median
```

---

# Case 2

\[
c=2
\]

Minimize:

\[
(a1-x)^2 + (a2-x)^2 ...
\]

Answer:

> Mean (Average)

---

Example

Numbers:

```text
[1,2,9,2,6]
```

Average:

```text
4
```

Minimum:

```text
46
```

---

## Rule

```text
Sum (ai−x)^2
```

↓

```text
Choose Mean
```

---

# 9. Common Greedy Patterns

---

## Sort + Pick

Examples:

- Activity Selection
- Job Scheduling
- Interval Problems

---

## Heap / Priority Queue

Examples:

- Huffman Coding
- Merge Cost

---

## Minimum Edge

Examples:

- Prim
- Kruskal

---

## Shortest Distance

Examples:

- Dijkstra

---

## Ratio Based

Examples:

- Fractional Knapsack

---

# 10. Greedy vs Dynamic Programming

| Greedy | DP |
|--------|----|
| Fast | Slower |
| Low memory | Higher memory |
| Local decisions | Global search |
| Not always optimal | Usually optimal |

---

# 11. How to Recognize Greedy in CP

Signals:

- Minimize / maximize
- Sorting seems natural
- Decision never needs undoing
- Local choice looks safe
- Intervals / events
- Earliest / smallest / largest
- Heap appears

Ask:

```text
Can I prove this choice is always safe?
```

If yes:

Try Greedy.

---


# Final Takeaway

> Greedy algorithms make the best decision available **right now**.

They are elegant and efficient.

But the hardest part is usually **proving that the greedy choice is correct**.