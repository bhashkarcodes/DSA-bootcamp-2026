# Hashing in Data Structures

Hashing is a technique used to store and retrieve data efficiently using a **hash function** that maps keys to specific positions in a data structure called a **hash table**.

- All three operations — **search, insert, and delete** — can be performed in **O(1)** time on average.
- Hashing is widely used to implement **sets** (distinct keys only) and **dictionaries/maps** (key-value pairs).

---

## Components of Hashing

| Component | Description |
|---|---|
| **Key** | The input value (string, integer, etc.) fed into the hash function |
| **Hash Function** | Takes the key and returns an index for the hash table |
| **Hash Table** | An array-based structure that stores values at computed indices |

---

## How Hashing Works

Consider this set of strings: `{ "al", "dm", "ghe" }`

We want to store these in a hash table of size **7** using a simple hash function.

**Step 1 — Map each character to a number** (a=1, b=2, ..., z=26)

**Step 2 — Sum the character values**

| String | Calculation | Sum |
|---|---|---|
| `"al"` | 1 + 12 | 13 |
| `"dm"` | 4 + 13 | 17 |
| `"ghe"` | 7 + 8 + 5 | 20 |

**Step 3 — Apply the hash function**

```
hash_index = sum % table_size
```

| String | Computation | Index |
|---|---|---|
| `"al"` | 13 % 7 | 6 |
| `"dm"` | 17 % 7 | 3 |
| `"ghe"` | 20 % 7 | 6 |

**Step 4 — Store in the table**

Both `"al"` and `"ghe"` map to index **6** — this is called a **collision**.

---

## Collision in Hashing

A **collision** occurs when two or more different keys produce the same hash index.

For example, `"ab"` and `"ba"` both have a character sum of 3, so they hash to the same index. Collisions are unavoidable in practice and must be handled using specific strategies (covered below).

---

## Load Factor

The **load factor** measures how full the hash table is:

```
Load Factor = Total elements in hash table / Size of hash table
```

It determines how efficiently the hash function distributes keys. A **higher load factor** means more collisions and slower performance.

---

## Rehashing

When the load factor exceeds a threshold (default: **0.75**), the hash table is **rehashed**:

1. A new array of **double the size** is created.
2. All existing elements are **re-inserted** using the new hash function.
3. This keeps the load factor low and maintains performance.

---

## HashMap (Key-Value Store)

A **HashMap** stores data as **key-value pairs**. It uses hashing internally to achieve average O(1) time for insert, delete, and lookup.

**Python**
```python
hashmap = {}

hashmap["apple"] = 3
hashmap["banana"] = 5
hashmap["orange"] = 2

print(hashmap["banana"])  # Output: 5

if "apple" in hashmap:
    print("Apple is present.")

del hashmap["orange"]

for key, value in hashmap.items():
    print(f"{key}: {value}")
```

**Java**
```java
import java.util.HashMap;

HashMap<String, Integer> hashmap = new HashMap<>();

hashmap.put("apple", 3);
hashmap.put("banana", 5);
hashmap.put("orange", 2);

System.out.println(hashmap.get("banana"));  // Output: 5

if (hashmap.containsKey("apple")) {
    System.out.println("Apple is present.");
}

hashmap.remove("orange");

for (var entry : hashmap.entrySet()) {
    System.out.println(entry.getKey() + ": " + entry.getValue());
}
```

**C++**
```cpp
#include <bits/stdc++.h>
using namespace std;

unordered_map<string, int> hashmap;

hashmap["apple"] = 3;
hashmap["banana"] = 5;
hashmap["orange"] = 2;

cout << hashmap["banana"] << endl;  // Output: 5

if (hashmap.count("apple")) {
    cout << "Apple is present." << endl;
}

hashmap.erase("orange");

for (auto& [key, value] : hashmap) {
    cout << key << ": " << value << endl;
}
```

---

## Index Mapping (Trivial Hashing)

**Index Mapping** is the simplest form of hashing where the key itself is used directly as the index. The hash function is essentially an identity function.

This works well when:
- Keys are integers within a known, bounded range.
- You need O(1) lookups with minimal overhead.

For negative numbers, their **absolute value** is used as the index, tracked in a separate column.

**Python**
```python
MAX = 1000
has = [[False, False] for _ in range(MAX + 1)]

def insert(arr):
    for x in arr:
        if x >= 0:
            has[x][0] = True
        else:
            has[abs(x)][1] = True

def search(x):
    if x >= 0:
        return has[x][0]
    return has[abs(x)][1]

arr = [-1, 9, -5, -8, -5, -2]
insert(arr)
print("Present" if search(-5) else "Not Present")  # Output: Present
```

**Java**
```java
boolean[][] has = new boolean[1001][2];

void insert(int[] arr) {
    for (int x : arr) {
        if (x >= 0) has[x][0] = true;
        else has[Math.abs(x)][1] = true;
    }
}

boolean search(int x) {
    if (x >= 0) return has[x][0];
    return has[Math.abs(x)][1];
}
```

**C++**
```cpp
#include <bits/stdc++.h>
using namespace std;
#define MAX 1000

bool has[MAX + 1][2];

void insert(int a[], int n) {
    for (int i = 0; i < n; i++) {
        if (a[i] >= 0) has[a[i]][0] = 1;
        else has[abs(a[i])][1] = 1;
    }
}

bool search(int X) {
    if (X >= 0) return has[X][0];
    return has[abs(X)][1];
}

int main() {
    int a[] = {-1, 9, -5, -8, -5, -2};
    int n = sizeof(a) / sizeof(a[0]);
    insert(a, n);
    cout << (search(-5) ? "Present" : "Not Present");
    return 0;
}
```

---

## Separate Chaining

**Separate Chaining** is a collision resolution technique where each index of the hash table holds a **linked list** (or chain) of all elements that hash to that index.

When a collision occurs, the new element is simply **appended to the list** at that index.

**How searching works:**
1. Compute the hash index for the key.
2. Traverse the chain at that index.
3. If the key is found, return it. If the chain ends without a match, the key is not present.

**Python**
```python
class HashMap:
    def __init__(self, size=10):
        self.size = size
        self.table = [[] for _ in range(size)]

    def hash(self, key):
        return hash(key) % self.size

    def insert(self, key, value):
        idx = self.hash(key)
        for pair in self.table[idx]:
            if pair[0] == key:
                pair[1] = value
                return
        self.table[idx].append([key, value])

    def search(self, key):
        idx = self.hash(key)
        for pair in self.table[idx]:
            if pair[0] == key:
                return pair[1]
        return None
```

**Java**
```java
import java.util.*;

class HashMap {
    int size = 10;
    LinkedList<int[]>[] table = new LinkedList[size];

    int hash(int key) { return key % size; }

    void insert(int key, int value) {
        int idx = hash(key);
        if (table[idx] == null) table[idx] = new LinkedList<>();
        for (int[] pair : table[idx]) {
            if (pair[0] == key) { pair[1] = value; return; }
        }
        table[idx].add(new int[]{key, value});
    }

    int search(int key) {
        int idx = hash(key);
        if (table[idx] == null) return -1;
        for (int[] pair : table[idx]) {
            if (pair[0] == key) return pair[1];
        }
        return -1;
    }
}
```

**C++**
```cpp
#include <bits/stdc++.h>
using namespace std;

class HashMap {
    int size = 10;
    vector<list<pair<int,int>>> table;
public:
    HashMap() : table(10) {}

    int hash(int key) { return key % size; }

    void insert(int key, int value) {
        int idx = hash(key);
        for (auto& p : table[idx]) {
            if (p.first == key) { p.second = value; return; }
        }
        table[idx].push_back({key, value});
    }

    int search(int key) {
        int idx = hash(key);
        for (auto& p : table[idx])
            if (p.first == key) return p.second;
        return -1;
    }
};
```

---

## Open Addressing

**Open Addressing** is a collision resolution method where all elements are stored **directly inside the hash table** itself — no external lists or chains are used.

When a collision occurs, the algorithm **probes** for the next available slot using a specific strategy:

| Strategy | Description |
|---|---|
| **Linear Probing** | Check the next slot one by one: `(hash + i) % size` |
| **Quadratic Probing** | Check slots at quadratic intervals: `(hash + i²) % size` |
| **Double Hashing** | Use a second hash function to determine step size |

Since all keys remain within the array, this method is also referred to as **closed hashing**.

**Python**
```python
class OpenHashMap:
    def __init__(self, size=10):
        self.size = size
        self.table = [None] * size

    def hash(self, key):
        return key % self.size

    def insert(self, key):
        idx = self.hash(key)
        while self.table[idx] is not None:
            idx = (idx + 1) % self.size  # Linear probing
        self.table[idx] = key

    def search(self, key):
        idx = self.hash(key)
        while self.table[idx] is not None:
            if self.table[idx] == key:
                return True
            idx = (idx + 1) % self.size
        return False
```

**Java**
```java
class OpenHashMap {
    int size = 10;
    Integer[] table = new Integer[size];

    int hash(int key) { return key % size; }

    void insert(int key) {
        int idx = hash(key);
        while (table[idx] != null) idx = (idx + 1) % size;
        table[idx] = key;
    }

    boolean search(int key) {
        int idx = hash(key);
        while (table[idx] != null) {
            if (table[idx] == key) return true;
            idx = (idx + 1) % size;
        }
        return false;
    }
}
```

**C++**
```cpp
#include <bits/stdc++.h>
using namespace std;

class OpenHashMap {
    int size = 10;
    vector<int> table;
public:
    OpenHashMap() : table(10, -1) {}

    int hash(int key) { return key % size; }

    void insert(int key) {
        int idx = hash(key);
        while (table[idx] != -1) idx = (idx + 1) % size;
        table[idx] = key;
    }

    bool search(int key) {
        int idx = hash(key);
        while (table[idx] != -1) {
            if (table[idx] == key) return true;
            idx = (idx + 1) % size;
        }
        return false;
    }
};
```

---

## Resources

- [Hashing Questions-GeeksforGeeks](http://geeksforgeeks.org/dsa/hashing-data-structure/)
- [Hashing — Maps, Collisions, Division Rule (Striver's A2Z DSA)](https://www.youtube.com/watch?v=KEs5UyBJ39g&t=2464s)
- [Implementing a Hash Table in C](https://youtu.be/2Ti5yvumFTU?t=3586)
- [Load Factor and Rehashing — GeeksforGeeks](https://www.geeksforgeeks.org/load-factor-and-rehashing/)
- [Separate Chaining — GeeksforGeeks](https://www.geeksforgeeks.org/separate-chaining-collision-handling-technique-in-hashing/)
- [Open Addressing — GeeksforGeeks](https://www.geeksforgeeks.org/open-addressing-collision-handling-technique-in-hashing/)
- [Index Mapping with Negatives — GeeksforGeeks](https://www.geeksforgeeks.org/index-mapping-or-trivial-hashing-with-negatives-allowed/)

