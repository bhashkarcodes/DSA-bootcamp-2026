# **Heap**

> 📌 **Week 6 — Study Material**

---

## **What is a Heap?**

A heap is a special **binary tree** stored in an array, where every parent node follows a strict ordering rule relative to its children:

- **Min-Heap** — every parent is **smaller** than its children (smallest element at the root)
- **Max-Heap** — every parent is **larger** than its children (largest element at the root)

Heaps are not fully sorted — they only guarantee the ordering between a parent and its direct children, not across the whole tree. This relaxed structure is exactly what makes heap operations fast.

---

## **Array Representation**

A heap is stored as an array, not as linked nodes. For a node at index `i`:

```
left child  = 2*i + 1
right child = 2*i + 2
parent      = (i - 1) / 2
```

```
        1
       / \
      3   5
     / \
    8   9

Array: [1, 3, 5, 8, 9]
Index:  0  1  2  3  4
```

No pointers needed — the index math alone navigates the tree. This is why heaps are memory-efficient compared to other tree structures.

---

## **Why Use a Heap?**

The heap gives you instant access to the minimum (or maximum) element while still letting you insert and remove efficiently — something a sorted array or plain array can't do well together.

| Operation | Sorted Array | Plain Array | Heap |
|-----------|-------------|-------------|------|
| Find min/max | O(1) | O(n) | O(1) |
| Insert | O(n) | O(1) | O(log n) |
| Remove min/max | O(n) | O(n) | O(log n) |

---

## **Operation 1 — Insertion**

Add the new element at the end of the array, then **bubble it up** until the heap property is restored.

**Example — Insert into Min-Heap:**

```cpp
void heapifyUp(vector<int>& heap, int i) {
    while (i > 0) {
        int parent = (i - 1) / 2;
        if (heap[i] < heap[parent]) {
            swap(heap[i], heap[parent]);   // fix violation
            i = parent;                     // move up
        } else {
            break;   // heap property satisfied, stop
        }
    }
}

void insert(vector<int>& heap, int val) {
    heap.push_back(val);              // add at the end
    heapifyUp(heap, heap.size() - 1); // restore heap property
}
```

**How we get O(log n):**
The new element can move up at most as many levels as the tree is deep. A heap with n elements has a height of **log₂(n)** (it's a complete binary tree, always balanced). So bubbling up takes at most log n swaps → **O(log n)**.

---

## **Operation 2 — Extract Min/Max**

Remove the root (min or max), move the **last element to the root**, then **bubble it down** until the heap property is restored.

**Example — Extract Min from Min-Heap:**

```cpp
void heapifyDown(vector<int>& heap, int i) {
    int n = heap.size();
    while (true) {
        int left = 2 * i + 1, right = 2 * i + 2;
        int smallest = i;

        if (left < n && heap[left] < heap[smallest])   smallest = left;
        if (right < n && heap[right] < heap[smallest])  smallest = right;

        if (smallest == i) break;   // heap property satisfied, stop

        swap(heap[i], heap[smallest]);
        i = smallest;                // move down
    }
}

int extractMin(vector<int>& heap) {
    int minVal = heap[0];
    heap[0] = heap.back();    // move last element to root
    heap.pop_back();
    heapifyDown(heap, 0);     // restore heap property
    return minVal;
}
```

**How we get O(log n):**
Same logic as insertion — the element being pushed down moves at most one step per level, and the tree height is log₂(n) → **O(log n)**.

---

## **Operation 3 — Build Heap from an Array**

Converting an unsorted array into a heap, by calling `heapifyDown` starting from the last non-leaf node up to the root.

```cpp
void buildHeap(vector<int>& arr) {
    int n = arr.size();
    for (int i = n / 2 - 1; i >= 0; i--) {   // start from last non-leaf node
        heapifyDown(arr, i);
    }
}
```

**How we get O(n), not O(n log n):**
It looks like n calls × O(log n) each = O(n log n), but this overestimates it. Most nodes are near the bottom of the tree, where `heapifyDown` does very little work — only nodes near the root sink far. Summing the actual work across all levels gives a tighter bound of **O(n)**.

---

## **Using the STL Priority Queue**

In competitive programming, you rarely build a heap by hand — C++'s `priority_queue` does it for you.

```cpp
#include <queue>

priority_queue<int> maxHeap;              // max-heap by default
maxHeap.push(5);
maxHeap.push(1);
maxHeap.push(8);
cout << maxHeap.top();                    // 8 (largest)
maxHeap.pop();                            // O(log n)

priority_queue<int, vector<int>, greater<int>> minHeap;  // min-heap
minHeap.push(5);
minHeap.push(1);
cout << minHeap.top();                    // 1 (smallest)
```

`push()` and `pop()` are both **O(log n)**; `top()` is **O(1)**.

---

## **Common Use Cases**

| Problem type | Why a heap fits |
|---------------|------------------|
| Kth largest/smallest element | Maintain a heap of size k, O(n log k) |
| Merge k sorted lists | Min-heap of current heads, always extract smallest |
| Median of a data stream | Two heaps (max-heap for lower half, min-heap for upper half) |
| Dijkstra's shortest path | Min-heap to always process the closest unvisited node |
| Task scheduling by priority | Max-heap pops the highest-priority task each time |

---

## **Key Tips**

- A heap is **not sorted** — only the parent-child relationship is guaranteed. Don't assume `heap[1]` is the second smallest.
- The heap is always a **complete binary tree** — all levels filled except possibly the last, filled left to right. This is what guarantees O(log n) height.
- Use `priority_queue<int, vector<int>, greater<int>>` for a min-heap — easy to forget the syntax, so keep it handy.
- For "find the Kth largest" type problems, keeping a heap of size **k** (not n) is the optimization that gets you O(n log k) instead of O(n log n).

---

## **Resources**

- [GeeksforGeeks — Heap Data Structure](https://www.geeksforgeeks.org/heap-data-structure/)
- [Visualgo — Heap Visualization](https://visualgo.net/en/heap)
- [LeetCode — Heap (Priority Queue) Problems](https://leetcode.com/tag/heap-priority-queue/)
