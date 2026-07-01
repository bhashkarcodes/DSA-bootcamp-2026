# Graph Representation

A **Graph** is a non-linear data structure consisting of:

- **Vertices (V)** (also called **nodes**)
- **Edges (E)** connecting pairs of vertices

A graph is denoted as:

\[
G(V, E)
\]

---

# Ways to Represent a Graph

The two most common graph representations are:

1. **Adjacency Matrix**
2. **Adjacency List**

---

# 1. Adjacency Matrix

An **Adjacency Matrix** represents a graph using a **2D matrix of size \(V \times V\)**.

- `matrix[i][j] = 1` → Edge exists from vertex **i** to **j**
- `matrix[i][j] = 0` → No edge exists

---

## Undirected Graph

For an undirected edge between `u` and `v`:

```text
matrix[u][v] = 1
matrix[v][u] = 1
```

The matrix is always **symmetric**.

### Example

Graph:

```
0 ----- 1
 \     /
  \   /
    2
```

Adjacency Matrix:

|   | 0 | 1 | 2 |
|---|---|---|---|
| **0** | 0 | 1 | 1 |
| **1** | 1 | 0 | 1 |
| **2** | 1 | 1 | 0 |

### Construction

Initially,

```cpp
0 0 0
0 0 0
0 0 0
```

Edges:

- 0–1
- 0–2
- 1–2

Update:

```cpp
mat[0][1] = mat[1][0] = 1;
mat[0][2] = mat[2][0] = 1;
mat[1][2] = mat[2][1] = 1;
```

---

## C++ (Undirected)

```cpp
#include <iostream>
#include <vector>
using namespace std;

vector<vector<int>> createGraph(int V, vector<vector<int>>& edges) {

    vector<vector<int>> mat(V, vector<int>(V, 0));

    for (auto &it : edges) {

        int u = it[0];
        int v = it[1];

        mat[u][v] = 1;
        mat[v][u] = 1;
    }

    return mat;
}

int main() {

    int V = 3;

    vector<vector<int>> edges = {
        {0,1},
        {0,2},
        {1,2}
    };

    vector<vector<int>> mat = createGraph(V, edges);

    cout << "Adjacency Matrix:\n";

    for(int i=0;i<V;i++) {
        for(int j=0;j<V;j++)
            cout << mat[i][j] << " ";
        cout << endl;
    }
}
```

---

## Python (Undirected)

```python
def createGraph(V, edges):

    mat = [[0]*V for _ in range(V)]

    for u, v in edges:
        mat[u][v] = 1
        mat[v][u] = 1

    return mat


V = 3

edges = [
    [0,1],
    [0,2],
    [1,2]
]

mat = createGraph(V, edges)

for row in mat:
    print(*row)
```

---

# Directed Graph

For a directed edge

```
u → v
```

Only

```cpp
matrix[u][v] = 1;
```

is updated.

Do **not** set

```cpp
matrix[v][u]
```

unless another edge exists.

---

### Example

Edges:

```
1 → 0
1 → 2
2 → 0
```

Adjacency Matrix:

|   |0|1|2|
|---|---|---|---|
|**0**|0|0|0|
|**1**|1|0|1|
|**2**|1|0|0|

---

# Complexity of Adjacency Matrix

| Operation | Complexity |
|------------|------------|
| Space | **O(V²)** |
| Add Edge | **O(1)** |
| Remove Edge | **O(1)** |
| Check Edge | **O(1)** |
| Iterate Neighbours | **O(V)** |

---

# 2. Adjacency List

Instead of a matrix, each vertex stores a list of its neighbours.

```text
adj[0] -> neighbours of 0
adj[1] -> neighbours of 1
...
adj[V-1]
```

Implemented using:

```cpp
vector<vector<int>>
```

---

## Undirected Graph

Whenever there is an edge

```
u — v
```

Store:

```cpp
adj[u].push_back(v);
adj[v].push_back(u);
```

---

### Example

Graph:

```
0 ----- 1
 \     /
  \   /
    2
```

Adjacency List:

```
0 -> 1 2
1 -> 0 2
2 -> 0 1
```

---

## C++ (Undirected)

```cpp
#include <iostream>
#include <vector>
using namespace std;

vector<vector<int>> createGraph(int V, vector<vector<int>>& edges) {

    vector<vector<int>> adj(V);

    for(auto &it : edges) {

        int u = it[0];
        int v = it[1];

        adj[u].push_back(v);
        adj[v].push_back(u);
    }

    return adj;
}

int main() {

    int V = 3;

    vector<vector<int>> edges = {
        {0,1},
        {0,2},
        {1,2}
    };

    vector<vector<int>> adj = createGraph(V, edges);

    cout << "Adjacency List\n";

    for(int i=0;i<V;i++) {

        cout << i << ": ";

        for(int x : adj[i])
            cout << x << " ";

        cout << endl;
    }
}
```

---

## Python (Undirected)

```python
def createGraph(V, edges):

    adj = [[] for _ in range(V)]

    for u, v in edges:
        adj[u].append(v)
        adj[v].append(u)

    return adj


V = 3

edges = [
    [0,1],
    [0,2],
    [1,2]
]

adj = createGraph(V, edges)

for i in range(V):
    print(i, ":", *adj[i])
```

---

# Directed Graph

For an edge

```
u → v
```

Only store:

```cpp
adj[u].push_back(v);
```

Do **not** insert `u` into `adj[v]`.

---

### Example

Edges:

```
1 → 0
1 → 2
2 → 0
```

Adjacency List:

```
0 ->
1 -> 0 2
2 -> 0
```

---

# Complexity of Adjacency List

| Operation | Complexity |
|------------|------------|
| Space | **O(V + E)** |
| Add Edge | **O(1)** |
| Remove Edge | **O(degree)** |
| Check Edge | **O(degree)** |
| Iterate Neighbours | **O(degree)** |

---

# Adjacency Matrix vs Adjacency List

| Feature | Adjacency Matrix | Adjacency List |
|---------|------------------|----------------|
| Space | O(V²) | O(V + E) |
| Check Edge | O(1) | O(degree) |
| Add Edge | O(1) | O(1) |
| Remove Edge | O(1) | O(degree) |
| Traverse Neighbours | O(V) | O(degree) |
| Best for | Dense Graphs | Sparse Graphs |

---

# When to Use Which?

### Use Adjacency Matrix when:

- Graph is **dense**
- Need **constant-time edge lookup**
- Number of vertices is small

### Use Adjacency List when:

- Graph is **sparse**
- Need to traverse neighbours efficiently
- Used in almost all competitive programming problems (BFS, DFS, Dijkstra, Topological Sort, etc.)

---

# Summary

- **Adjacency Matrix** stores every possible edge using a 2D matrix.
- **Adjacency List** stores only existing neighbours.
- Matrix uses **O(V²)** space.
- List uses **O(V + E)** space.
- Adjacency Lists are generally preferred in practice due to lower memory usage and efficient traversal.

# Breadth First Search (BFS)

**Breadth First Search (BFS)** is a graph traversal algorithm that starts from a source vertex and explores the graph **level by level**.

Instead of going as deep as possible (like DFS), BFS first visits **all immediate neighbours**, then their neighbours, and so on until every reachable vertex has been visited.

---

## Key Characteristics

- Traverses the graph **level-wise**.
- Uses a **Queue (FIFO)**.
- Visits the **closest vertices first**.
- Guarantees the **shortest path in an unweighted graph**.
- Can be applied to both **directed** and **undirected** graphs.

---

# Working of BFS

Suppose we start from source vertex `0`.

Algorithm:

1. Mark source as visited.
2. Push source into the queue.
3. While queue is not empty:
   - Remove the front vertex.
   - Visit it.
   - Push all its unvisited neighbours into the queue.

---

# Example 1

Adjacency List

```cpp
0 -> 1 2
1 -> 0 2
2 -> 0 1 3 4
3 -> 2
4 -> 2
```

Traversal starting from **0**

```
Queue = [0]

Visit 0
Queue = [1,2]

Visit 1
Queue = [2]

Visit 2
Queue = [3,4]

Visit 3
Queue = [4]

Visit 4
Queue = []
```

Output

```
0 1 2 3 4
```

---

# Example 2 (Disconnected Graph)

Adjacency List

```cpp
0 -> 2 3
1 -> 2
2 -> 0 1
3 -> 0

4 -> 5
5 -> 4
```

Starting BFS from `0`

```
0 2 3 1
```

Vertices `4` and `5` are never reached because they belong to another connected component.

To traverse the **entire graph**, run BFS again from every unvisited vertex.

Final traversal:

```
0 2 3 1 4 5
```

---

# BFS Algorithm (Single Connected Component)

### C++

```cpp
#include <vector>
#include <queue>
using namespace std;

vector<int> bfs(vector<vector<int>>& adj) {

    int V = adj.size();

    vector<bool> visited(V, false);
    vector<int> res;

    queue<int> q;

    int src = 0;

    visited[src] = true;
    q.push(src);

    while (!q.empty()) {

        int curr = q.front();
        q.pop();

        res.push_back(curr);

        for (int x : adj[curr]) {

            if (!visited[x]) {

                visited[x] = true;
                q.push(x);

            }
        }
    }

    return res;
}
```

---

### Python

```python
from collections import deque

def bfs(adj):

    V = len(adj)

    visited = [False] * V
    res = []

    src = 0

    q = deque()

    visited[src] = True
    q.append(src)

    while q:

        curr = q.popleft()
        res.append(curr)

        for x in adj[curr]:

            if not visited[x]:

                visited[x] = True
                q.append(x)

    return res
```

---

# BFS for a Disconnected Graph

If the graph has multiple connected components, starting BFS from a single source will not visit every vertex.

The solution is simple:

- Iterate through every vertex.
- If it hasn't been visited, perform a BFS from that vertex.

This ensures every connected component is explored.

---

## C++

```cpp
#include <vector>
#include <queue>
using namespace std;

void bfsConnected(vector<vector<int>>& adj,
                  int src,
                  vector<bool>& visited,
                  vector<int>& res) {

    queue<int> q;

    visited[src] = true;
    q.push(src);

    while (!q.empty()) {

        int curr = q.front();
        q.pop();

        res.push_back(curr);

        for (int x : adj[curr]) {

            if (!visited[x]) {

                visited[x] = true;
                q.push(x);

            }
        }
    }
}

vector<int> bfs(vector<vector<int>>& adj) {

    int V = adj.size();

    vector<bool> visited(V, false);
    vector<int> res;

    for (int i = 0; i < V; i++) {

        if (!visited[i]) {

            bfsConnected(adj, i, visited, res);

        }
    }

    return res;
}
```

---

## Python

```python
from collections import deque

def bfsConnected(adj, src, visited, res):

    q = deque()

    visited[src] = True
    q.append(src)

    while q:

        curr = q.popleft()
        res.append(curr)

        for x in adj[curr]:

            if not visited[x]:

                visited[x] = True
                q.append(x)


def bfs(adj):

    V = len(adj)

    visited = [False] * V
    res = []

    for i in range(V):

        if not visited[i]:

            bfsConnected(adj, i, visited, res)

    return res
```

---

# Time Complexity

Let

- **V** = Number of vertices
- **E** = Number of edges

| Operation | Complexity |
|------------|------------|
| BFS Traversal | **O(V + E)** |
| Space (Visited + Queue) | **O(V)** |

Each vertex is visited **once**, and every edge is processed **once** (twice in an undirected graph, which is still `O(E)`).

---

# Why BFS is O(V + E)?

- Every vertex enters the queue only once.
- Every edge is examined only once (or twice for undirected graphs).
- Therefore,

```
Total Work = O(V) + O(E)
           = O(V + E)
```

---

# Applications of BFS

- Shortest Path in an **Unweighted Graph**
- Cycle Detection
- Finding Connected Components
- Level Order Traversal
- Network Routing
- Social Network Analysis
- Web Crawlers
- Broadcasting in Networks
- Bipartite Graph Checking

---

# BFS vs DFS

| Feature | BFS | DFS |
|---------|-----|-----|
| Data Structure | Queue | Stack / Recursion |
| Traversal | Level by Level | Depth First |
| Shortest Path (Unweighted) | ✅ Yes | ❌ No |
| Memory Usage | Higher | Lower (typically) |
| Used In | Shortest Path, Levels | Topological Sort, SCC, Backtracking |

---

# Summary

- BFS explores vertices **level by level**.
- Uses a **Queue (FIFO)**.
- Time Complexity is **O(V + E)**.
- Space Complexity is **O(V)**.
- Finds the **shortest path in unweighted graphs**.
- For disconnected graphs, run BFS from every unvisited vertex.

# Depth First Search (DFS)

**Depth First Search (DFS)** is a graph traversal algorithm that starts from a source vertex and explores **one path as deeply as possible** before backtracking to explore other paths.

Unlike trees, graphs may contain **cycles**, so we maintain a **visited array** to avoid visiting the same vertex multiple times.

> **Note:** A graph can have multiple valid DFS traversals depending on the order in which neighbours are visited.

---

# Key Characteristics

- Traverses one branch completely before moving to another.
- Uses **Recursion** (or an explicit **Stack**).
- Requires a **visited array** for graphs.
- Does **not** guarantee the shortest path.
- Can be applied to both **directed** and **undirected** graphs.

---

# Working of DFS

Suppose we start from source vertex `0`.

Algorithm:

1. Mark the current vertex as visited.
2. Visit the current vertex.
3. Recursively visit every unvisited neighbour.
4. If no unvisited neighbour exists, backtrack to the previous vertex.

---

# Example 1

Adjacency List

```cpp
0 -> 1 2
1 -> 0 2
2 -> 0 1 3 4
3 -> 2
4 -> 2
```

Starting from vertex **0**

```
Visit 0

Go to 1

Go to 2

Go to 3

Backtrack to 2

Go to 4

Backtrack to 2

Backtrack to 1

Backtrack to 0
```

Traversal

```
0 1 2 3 4
```

---

# Example 2 (Disconnected Graph)

Adjacency List

```cpp
0 -> 2 3
1 -> 2
2 -> 0 1
3 -> 0

4 -> 5
5 -> 4
```

Starting from `0`

```
0 2 1 3
```

Vertices `4` and `5` are never reached because they belong to another connected component.

Start another DFS from `4`.

Final traversal

```
0 2 1 3 4 5
```

---

# DFS Algorithm (Single Connected Component)

## C++

```cpp
#include <vector>
using namespace std;

void dfsRec(vector<vector<int>>& adj,
            vector<bool>& visited,
            int s,
            vector<int>& res) {

    visited[s] = true;

    res.push_back(s);

    for (int x : adj[s]) {

        if (!visited[x]) {

            dfsRec(adj, visited, x, res);

        }
    }
}

vector<int> dfs(vector<vector<int>>& adj) {

    vector<bool> visited(adj.size(), false);
    vector<int> res;

    dfsRec(adj, visited, 0, res);

    return res;
}
```

---

## Python

```python
def dfsRec(adj, visited, s, res):

    visited[s] = True

    res.append(s)

    for x in adj[s]:

        if not visited[x]:

            dfsRec(adj, visited, x, res)


def dfs(adj):

    visited = [False] * len(adj)
    res = []

    dfsRec(adj, visited, 0, res)

    return res
```

---

# DFS for a Disconnected Graph

If the graph has multiple connected components, starting DFS from one source will not visit every vertex.

To visit the entire graph:

- Iterate through every vertex.
- If it hasn't been visited, start a DFS from that vertex.

This explores every connected component.

---

## C++

```cpp
#include <vector>
using namespace std;

void dfsRec(vector<vector<int>>& adj,
            vector<bool>& visited,
            int s,
            vector<int>& res) {

    visited[s] = true;

    res.push_back(s);

    for (int x : adj[s]) {

        if (!visited[x]) {

            dfsRec(adj, visited, x, res);

        }
    }
}

vector<int> dfs(vector<vector<int>>& adj) {

    int V = adj.size();

    vector<bool> visited(V, false);
    vector<int> res;

    for (int i = 0; i < V; i++) {

        if (!visited[i]) {

            dfsRec(adj, visited, i, res);

        }
    }

    return res;
}
```

---

## Python

```python
def dfsRec(adj, visited, s, res):

    visited[s] = True

    res.append(s)

    for x in adj[s]:

        if not visited[x]:

            dfsRec(adj, visited, x, res)


def dfs(adj):

    V = len(adj)

    visited = [False] * V
    res = []

    for i in range(V):

        if not visited[i]:

            dfsRec(adj, visited, i, res)

    return res
```

---

# Time Complexity

Let

- **V** = Number of vertices
- **E** = Number of edges

| Operation | Complexity |
|------------|------------|
| DFS Traversal | **O(V + E)** |
| Space (Visited + Recursion Stack) | **O(V)** |

Each vertex is visited exactly once, and every edge is explored once (twice in an undirected graph, still `O(E)`).

---

# Why DFS is O(V + E)?

- Every vertex is visited once.
- Every edge is explored once (or twice in undirected graphs).
- Therefore,

```
Total Work = O(V) + O(E)
           = O(V + E)
```

---

# Recursive Nature of DFS

Suppose the graph is

```
0
|
1
|
2
|
3
```

Recursive calls look like

```
dfs(0)
    dfs(1)
        dfs(2)
            dfs(3)
```

When `3` finishes,

```
dfs(3) returns

↓

dfs(2) returns

↓

dfs(1) returns

↓

dfs(0) returns
```

This returning process is called **backtracking**.

---

# Iterative DFS (Using Stack)

DFS can also be implemented without recursion using a stack.

### C++

```cpp
#include <vector>
#include <stack>
using namespace std;

vector<int> dfs(vector<vector<int>>& adj) {

    int V = adj.size();

    vector<bool> visited(V, false);
    vector<int> res;

    stack<int> st;

    st.push(0);

    while (!st.empty()) {

        int curr = st.top();
        st.pop();

        if (visited[curr])
            continue;

        visited[curr] = true;
        res.push_back(curr);

        for (int i = adj[curr].size() - 1; i >= 0; i--) {

            int x = adj[curr][i];

            if (!visited[x])
                st.push(x);
        }
    }

    return res;
}
```

---

# Applications of DFS

- Cycle Detection
- Connected Components
- Topological Sorting
- Strongly Connected Components (Kosaraju, Tarjan)
- Bridge Detection
- Articulation Points
- Maze Solving
- Path Finding
- Backtracking Problems
- Tree Traversals

---

# BFS vs DFS

| Feature | BFS | DFS |
|---------|-----|-----|
| Data Structure | Queue | Stack / Recursion |
| Traversal | Level by Level | Depth First |
| Shortest Path (Unweighted) | ✅ Yes | ❌ No |
| Memory Usage | Higher | Lower (typically) |
| Backtracking | ❌ No | ✅ Yes |
| Common Uses | Shortest Path, Levels | SCC, Topological Sort, Bridges |

---

# Summary

- DFS explores one path **as deeply as possible** before backtracking.
- Uses **Recursion** or a **Stack**.
- Requires a **visited array** for graphs.
- Time Complexity is **O(V + E)**.
- Space Complexity is **O(V)**.
- For disconnected graphs, start DFS from every unvisited vertex.
- DFS is widely used in graph algorithms such as topological sorting, SCCs, bridges, articulation points, and backtracking problems.

# Essential Graph Problems Roadmap

---

# 1. Traversal

| Problem | Concepts | Link |
|---------|----------|------|
| BFS of Graph | Queue, Level-order traversal |https://www.geeksforgeeks.org/problems/bfs-traversal-of-graph/1 |
| DFS of Graph | Recursion, Backtracking |https://www.geeksforgeeks.org/problems/depth-first-traversal-for-a-graph/1 |

---

# 2. Grid Graphs (BFS / DFS)

| Problem | Concepts | Link |
|---------|----------|------|
| Flood Fill | DFS/BFS on Matrix | https://www.geeksforgeeks.org/dsa/flood-fill-algorithm/|
| Rotten Tomatoes | Multi-source BFS | https://www.geeksforgeeks.org/dsa/minimum-time-required-so-that-all-oranges-become-rotten/|
| Number of Islands | Connected Components in Grid |https://www.geeksforgeeks.org/dsa/find-the-number-of-islands-using-dfs/ |

---

# 3. Graph Coloring

| Problem | Concepts | Link |
|---------|----------|------|
| Check if Graph is Bipartite | BFS/DFS Coloring | https://www.geeksforgeeks.org/dsa/bipartite-graph/|

---

# 4. Cycle Detection

| Problem | Concepts | Link |
|---------|----------|------|
| Detect Cycle in Undirected Graph | BFS/DFS, Parent Tracking |https://www.geeksforgeeks.org/dsa/detect-cycle-undirected-graph/ |
| Detect Cycle in Directed Graph | DFS, Recursion Stack |https://www.geeksforgeeks.org/dsa/detect-cycle-in-a-graph/ |

---

# 5. Topological Sorting

| Problem | Concepts | Link |
|---------|----------|------|
| Topological Sort (DFS) | DFS Finish Order |https://www.geeksforgeeks.org/dsa/topological-sorting/ |
| Kahn's Algorithm | BFS, Indegree | https://www.geeksforgeeks.org/dsa/topological-sorting-indegree-based-solution/|

---

# 6. Shortest Paths

| Problem | Concepts | Link |
|---------|----------|------|
| Shortest Path in Unweighted Graph | BFS | https://www.geeksforgeeks.org/dsa/shortest-path-unweighted-graph/|
| Dijkstra's Algorithm | Priority Queue | https://www.geeksforgeeks.org/dsa/dijkstras-shortest-path-algorithm-greedy-algo-7/|
| Bellman-Ford Algorithm | Negative Edge Weights |https://www.geeksforgeeks.org/dsa/bellman-ford-algorithm-dp-23/ |
| Floyd-Warshall Algorithm | All-Pairs Shortest Path |https://www.geeksforgeeks.org/dsa/floyd-warshall-algorithm-dp-16/ |

---

# 7. Minimum Spanning Tree (MST)

| Problem | Concepts | Link |
|---------|----------|------|
| Prim's Algorithm | Greedy, Priority Queue | https://www.geeksforgeeks.org/dsa/prims-minimum-spanning-tree-mst-greedy-algo-5/|
| Kruskal's Algorithm | Greedy, Disjoint Set Union (DSU) |https://www.geeksforgeeks.org/dsa/kruskals-minimum-spanning-tree-algorithm-greedy-algo-2/ |

---
