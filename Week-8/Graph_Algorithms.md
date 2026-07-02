## ADVANCED GRAPH ALGORITHMS 

Welcome to the comprehensive guide for the Advanced Graph Algorithms Module . 

## MODULE 1: TOPOLOGICAL SORTING & DIRECTED ACYCLIC GRAPHS (DAGS) 

## 1. TOPOLOGICAL SORT (DFS-BASED) 


## Intuition 

A Directed Acyclic Graph (DAG) represents a system of unidirectional constraints or dependencies. Think of a topological sort as finding a linear ordering of vertices such that for every directed edge u→v, vertex u comes before v in the ordering. If there is a cycle, such an ordering is impossible because dependencies become circular.  

## Real-World Analogy 

Consider a college degree path. You cannot take _Advanced Algorithms_ without completing _Data Structures_ , and you cannot take _Data Structures_ without completing _Introduction to Programming_ . A topological sort provides a valid sequence of classes to graduate semester-by-semester without breaking any prerequisite rules. 

## Core Theory & Algorithm 

The DFS-based approach relies on the properties of a recursive call stack. 

1. We maintain a `visited` array to keep track of explored nodes. 

2. We initiate a standard DFS traversal for each unvisited node. 

3. For a given node u, we recursively visit all its unvisited neighbors v. 

4. The Core Trick: Only _after_ all neighbors of u have been completely processed and backtracked from, we push u onto a tracking stack. 

5. Once all nodes are visited, the stack contains the topological order from top to bottom (reversing the completion order ensures prerequisites appear first). 

## Dry Run 

- Let a graph have edges: `1 -> 2` , `2 -> 3` , `1 -> 3` . 

   - Start DFS at `1` . Visited: `{1}` . 

   - Move to neighbor `2` . Visited: `{1, 2}` . 

   - Move to neighbor `3` . Visited: `{1, 2, 3}` . 

   - Node `3` has no outgoing edges. It finishes. Push `3` to Stack. Stack: `[3]` 

   - Backtrack to `2` . Node `2` has no other unvisited neighbors. It finishes. Push `2` to Stack. Stack: `[3, 2]` 

   - Backtrack to `1` . Neighbor `3` is already visited. Node `1` finishes. Push `1` to Stack. Stack: `[3, 2, 1]` 

   - Popping from the stack yields: `1, 2, 3` . (Valid ordering). 

## Implementations 

Below are clean implementations in C++, Java, and Python. 

C++ Implementation 

C++ 

```
#include <iostream>
#include <vector>
#include <stack>

using namespace std;

void dfs(int node, const vector<vector<int>>& adj, vector<bool>&
visited, stack<int>& st) {
    visited[node] = true;
    for (int neighbor : adj[node]) {
        if (!visited[neighbor]) {
            dfs(neighbor, adj, visited, st);
        }
    }
    st.push(node);
}
vector<int> topologicalSort(int v, const vector<vector<int>>& adj) {
    vector<bool> visited(v, false);
    stack<int> st;
    for (int i = 0; i < v; i++) {
        if (!visited[i]) {
            dfs(i, adj, visited, st);
        }
    }
    vector<int> order;
    while (!st.empty()) {
        order.push_back(st.top());
        st.pop();
    }
    return order;
}
```

## Java Implementation 

Java 

```
import java.util.*;

public class TopologicalSortDFS {
    private static void dfs(int node, List<List<Integer>> adj, boolean[]
visited, Stack<Integer> stack) {
        visited[node] = true;
        for (int neighbor : adj.get(node)) {
            if (!visited[neighbor]) {
                dfs(neighbor, adj, visited, stack);
            }
        }
        stack.push(node);
    }
    public static List<Integer> topologicalSort(int v,
List<List<Integer>> adj) {
        boolean[] visited = new boolean[v];
        Stack<Integer> stack = new Stack<>();
        for (int i = 0; i < v; i++) {
            if (!visited[i]) {
                dfs(i, adj, visited, stack);
            }
        }
        List<Integer> order = new ArrayList<>();
        while (!stack.isEmpty()) {
            order.add(stack.pop());
        }
        return order;
    }
}
```

## Python Implementation 

## Python 

```
def topological_sort_dfs(v: int, adj: list[list[int]]) -> list[int]:
    visited = [False] * v
    stack = []
    def dfs(node: int):
        visited[node] = True
        for neighbor in adj[node]:
            if not visited[neighbor]:

                dfs(neighbor)
        stack.append(node)

    for i in range(v):
        if not visited[i]:
            dfs(i)
    return stack[::-1]
```

## Complexity Analysis 

- Time Complexity: O(V+E) where V is the number of vertices and E is the number of edges. We visit every vertex once and iterate through all adjacency lists. 
 

- Space Complexity: O(V) for the recursion call stack, the `visited` tracker array, and the output sequence stack. 

## Common Mistakes 

- Ignoring Cycles: Standard DFS topo-sort will output a completely broken, incorrect sequence if run on a graph containing cycles without explicit cycle detection logic. 

- Disconnected Components: Forgetting to iterate from `0` to `V - 1` in the outer loop, which skips isolated subgraphs. 
 

## 2. KAHN'S ALGORITHM (BFS-BASED) 

## Intuition 

If a task has no prerequisites, you can complete it immediately. Kahn’s algorithm iteratively uncovers nodes that have an in-degree (number of incoming directed edges) of `0` , processes them, and clears their outgoing dependencies. 

## Real-World Analogy 

Imagine a manufacturing assembly line. You look for any parts that don't require preassembly. You build those first. Once built, you cross them off your list, which frees up other parts that were waiting on them. 

## Core Theory & Algorithm 

1. Calculate the in-degree of all vertices. 

2. Add all vertices with an `in-degree == 0` to a standard FIFO queue. 

3. Pop a vertex u from the queue and append it to your output list. 

4. For every directed neighbor v of u, decrement its in-degree by `1` . 

5. If the in-degree of v drops to `0` , push it immediately into the queue. 

6. Repeat until the queue is empty. If the size of your final output list is less than V, the graph contains a cycle (detecting circular dependencies). 

## Dry Run 

Graph: `0 -> 2` , `1 -> 2` , `2 -> 3` . 

- Initial In-degrees: `[0:0, 1:0, 2:2, 3:1]` 

- Initial Queue: `[0, 1]` (nodes with in-degree 0). 

- Pop `0` : Output: `[0]` . Decrement neighbor `2` . In-degrees: `[1:0, 2:1,3:1]` . 

- Pop `1` : Output: `[0, 1]` . Decrement neighbor `2` . In-degree of `2` becomes `0` . 

Push `2` to Queue. Queue: `[2]` . 

- Pop `2` : Output: `[0, 1, 2]` . Decrement neighbor`3` . In-degree of `3` becomes `0` . Push `3` to Queue. Queue: `[3]` . 

- Pop `3` : Output: `[0, 1, 2, 3]` . Queue empty. Done. 

## Implementations 

## C++ Implementation 

C++ 

```
#include <iostream>
#include <vector>
#include <queue>

using namespace std;

vector<int> kahnsAlgorithm(int v, const vector<vector<int>>& adj) {
    vector<int> in_degree(v, 0);
    for (int i = 0; i < v; i++) {
        for (int neighbor : adj[i]) {
            in_degree[neighbor]++;
        }
    }
    queue<int> q;
    for (int i = 0; i < v; i++) {
        if (in_degree[i] == 0) q.push(i);
    }
    vector<int> order;
    while (!q.empty()) {
        int u = q.front();
        q.pop();
        order.push_back(u);

        for (int v_node : adj[u]) {

    if (order.size() != v) return {}; // Cycle detected
    return order;
}
```

## Java Implementation 

Java 

```
import java.util.*;

public class KahnsAlgorithm {
    public static List<Integer> kahnsAlgorithm(int v,
List<List<Integer>> adj) {
        int[] inDegree = new int[v];
        for (int i = 0; i < v; i++) {
            for (int neighbor : adj.get(i)) {
                inDegree[neighbor]++;
            }
        }

        Queue<Integer> q = new LinkedList<>();
        for (int i = 0; i < v; i++) {
            if (inDegree[i] == 0) q.add(i);
        }

        List<Integer> order = new ArrayList<>();
        while (!q.isEmpty()) {
            int u = q.poll();
            order.add(u);

        }

        if (order.size() != v) return new ArrayList<>(); // Cycle
detected
        return order;
    }
}
```

## Python Implementation 

## Python 

```
from collections import deque
def kahns_algorithm(v: int, adj: list[list[int]]) -> list[int]:
    in_degree = [0] * v
    for u in range(v):
        for neighbor in adj[u]:
            in_degree[neighbor] += 1
    q = deque([i for i in range(v) if in_degree[i] == 0])
    order = []
    while q:
        u = q.popleft()
        order.append(u)
        for neighbor in adj[u]:
            in_degree[neighbor] -= 1
            if in_degree[neighbor] == 0:
                q.append(neighbor)
    if len(order) != v:
        return [] # Cycle detected
    return order
```

## Complexity Analysis 

- Time Complexity: O(V+E). We scan all nodes to calculate in-degree and process each vertex and edge exactly once via the queue.  

- Space Complexity: O(V) to store the array tracking incoming degrees and the queue structure. 


## MODULE 1 PRACTICE PROBLEMS 

- Easy: LeetCode 207 - Course Schedule 

- Medium: LeetCode 210 - Course Schedule II  

- Hard: LeetCode 269 - Alien Dictionary 

## MODULE 2: SHORTEST PATH ALGORITHMS 

## 1. DIJKSTRA’S ALGORITHM 

## Intuition 

Dijkstra's algorithm solves the Single-Source Shortest Path (SSSP) problem on graphs with non-negative edge weights. It uses a greedy approach: always expand the path that currently has the minimum accumulated absolute distance from the source node. 

## Real-World Analogy 

gine ripples of water spreading out from a single point across an irregular grid of pipes. The water naturally flows through the shortest, least-resisting channels first, reaching closer intersections before distant ones. 

## Core Theory & Algorithm 

Dijkstra maintains a tentative distance array `dist` , initialized to ∞, except for 

`dist[source] = 0` . 

1. Insert `{0, source}` into a min-priority queue (ordered by distance). 

2. Extract the element `{d, u}` with the minimum distance. 

3. If d>dist[u], discard it as stale. 

4. For each neighbor v connected to u by weight w: 

   - If dist[u]+w<dist[v]: 

Update dist[v]=dist[u]+w(Relaxation step) [cite: 35] Push `{dist[v], v}` into the min-priority queue. 

## Why Negative Weights Cause Dijkstra to Fail 

Dijkstra operates on a strict greedy principle: once a node is extracted from the priority queue, its shortest distance is assumed to be finalized. If negative edge weights exist, a longer structural path could later decrease its total cost, invalidating the early greedy choices. 


## Implementations 

C++ Implementation 

C++ 

```
#include <iostream>
#include <vector>
#include <queue>
using namespace std;
const int INF = 1e9;

vector<int> dijkstra(int source, int v, const vector<vector<pair<int,
int>>>& adj) {
    vector<int> dist(v, INF);
    priority_queue<pair<int, int>, vector<pair<int, int>>,
greater<pair<int, int>>> pq;
    dist[source] = 0;
    pq.push({0, source});
    while (!pq.empty()) {
        auto [d, u] = pq.top();
        pq.pop();
        if (d > dist[u]) continue;
        for (auto& edge : adj[u]) {

            int target = edge.first;
            int weight = edge.second;
            if (dist[u] + weight < dist[target]) {
                dist[target] = dist[u] + weight;
                pq.push({dist[target], target});
            }
        }
    }
    return dist;
}
```

## Java Implementation 

Java 

```
import java.util.*;
public class Dijkstra {
    private static final int INF = (int) 1e9;
    public static int[] dijkstra(int source, int v, List<List<int[]>>
adj) {
        int[] dist = new int[v];
        Arrays.fill(dist, INF);
        PriorityQueue<int[]> pq = new PriorityQueue<>
(Comparator.comparingInt(a -> a[0]));

        pq.add(new int[]{0, source});
            int[] curr = pq.poll();
            if (d > dist[u]) continue;
            for (int[] edge : adj.get(u)) {
                int target = edge[0];
                int weight = edge[1];

                if (dist[u] + weight < dist[target]) {
                    dist[target] = dist[u] + weight;
                    pq.add(new int[]{dist[target], target});
                }
            }
        }
        return dist;
    }
}
```

## Python Implementation 

Python 

```
import heapq
INF = float('inf')
def dijkstra(source: int, v: int, adj: list[list[tuple[int, int]]]) ->
list[int]:
    dist = [INF] * v
    dist[source] = 0
    pq = [(0, source)]  # (distance, node)
    while pq:
        d, u = heapq.heappop(pq)
        if d > dist[u]:
            continue
        for target, weight in adj[u]:
            if dist[u] + weight < dist[target]:
                dist[target] = dist[u] + weight
                heapq.heappush(pq, (dist[target], target))
    return dist
```

## Complexity Analysis 

- Time Complexity: O((V+E)logV). Every edge is relaxed once, and priority queue mutations take logarithmic time. 

- Space Complexity: O(V+E) to maintain the distance tracking maps along with the elements in the heap structures. 

## 2. BELLMAN-FORD ALGORITHM 

## Intuition 

If a graph contains negative edge weights , Dijkstra's greedy strategy fails. Bellman- 
Ford takes a brute-force approach to path minimization: it relaxes _all_ edges in the graph V−1 times. 

## Core Theory & Algorithm 

imple path in a graph with V vertices can contain at most V−1 edges without creating cycles. Therefore, relaxing all edges V−1 times guarantees that the shortest paths propagate across the entire network. 


1. Initialize `dist` array to ∞, `dist[source] = 0` . 

2. Loop V−1 times: 

   - For every directed edge u→v with weight w: 

$$\text{if } \text{dist}[u] \neq \infty \text{ and } \text{dist}[u] + w < \text{dist}[v]: \text{dist}[v] = \text{dist}[u] + w \text{ [cite: 35]}$$

3. Negative Cycle Detection Step: Run the relaxation loop one more time. If any `dist[v]` decreases further, a negative cycle exists because the path length 

keeps dropping infinitely.  

## Implementations 

## C++ Implementation 

C++ 

```
#include <iostream>
#include <vector>
using namespace std;
const int INF = 1e9;
struct Edge { int u, v, w; };

bool bellmanFord(int source, int v, int e, const vector<Edge>& edges,
vector<int>& dist) {
    dist.assign(v, INF);
    dist[source] = 0;
    for (int i = 1; i <= v - 1; i++) {
        for (const auto& edge : edges) {
            if (dist[edge.u] != INF && dist[edge.u] + edge.w <
dist[edge.v]) {
                dist[edge.v] = dist[edge.u] + edge.w;
            }
        }
    }
    // Check for negative weight cycles
    for (const auto& edge : edges) {
        if (dist[edge.u] != INF && dist[edge.u] + edge.w < dist[edge.v])
{
            return true; // Negative cycle detected
        }
    }

    return false;
}
```

## Java Implementation 

Java 

```
import java.util.*;

public class BellmanFord {
    private static final int INF = (int) 1e9;
    static class Edge {
        int u, v, w;
        Edge(int u, int v, int w) { this.u = u; this.v = v; this.w = w;
}
    }

    public static boolean bellmanFord(int source, int v, List<Edge>edges, int[] dist) {
        Arrays.fill(dist, INF);
        dist[source] = 0;
        for (int i = 1; i < v; i++) {
            for (Edge edge : edges) {
                if (dist[edge.u] != INF && dist[edge.u] + edge.w <dist[edge.v]) {
                    dist[edge.v] = dist[edge.u] + edge.w;
                }
            }
        }
        for (Edge edge : edges) {
            if (dist[edge.u] != INF && dist[edge.u] + edge.w <dist[edge.v]) {
                return true; // Negative cycle detected
            }
        }
        return false;
    }
}
```

## Python Implementation 

## Python 

```
INF = float('inf')

def bellman_ford(source: int, v: int, edges: list[tuple[int, int, int]])
-> tuple[list[int], bool]:
    dist = [INF] * v
    dist[source] = 0
    for _ in range(v - 1):
        for u, val_v, w in edges:
            if dist[u] != INF and dist[u] + w < dist[val_v]:
                dist[val_v] = dist[u] + w
    # Detect negative cycle
    for u, val_v, w in edges:
        if dist[u] != INF and dist[u] + w < dist[val_v]:
            return [], True # Negative cycle exists
    return dist, False
```

## Complexity Analysis 

Time Complexity: O(V×E). We perform V−1 structural passes over all E edges.  

Space Complexity: O(V) to maintain the single-source distance tracking array. 

## 3. FLOYD-WARSHALL ALGORITHM 

## Intuition 

Floyd-Warshall is an All-Pairs Shortest Path (APSP) algorithm. It uses dynamic programming to find the shortest path between _any_ two nodes i and j, evaluating whether routing through an intermediate node k yields a shorter path. 

## Core Theory & Equation 

 matrix[i][j] be the shortest distance between node i and j. For every node k, we update all pairs (i,j): 

## matrix[i][j]=min(matrix[i][j],matrix[i][k]+matrix[k][j]) [cite: 40] 

## Implementations 

## C++ Implementation 

C++ 

```
#include <vector>
#include <algorithm>
using namespace std;
const int INF = 1e9;
void floydWarshall(int v, vector<vector<int>>& matrix) {
    for (int k = 0; i < v; k++) {
        for (int i = 0; i < v; i++) {
            for (int j = 0; j < v; j++) {
                if (matrix[i][k] != INF && matrix[k][j] != INF) {
                    matrix[i][j] = min(matrix[i][j], matrix[i][k] +matrix[k][j]);
                }
            }
        }
    }
}
```

## Python Implementation 

Python 

```
INF = float('inf')

def floyd_warshall(v: int, matrix: list[list[int]]):
    for k in range(v):
        for i in range(v):
            for j in range(v):
                if matrix[i][k] != INF and matrix[k][j] != INF:
                    matrix[i][j] = min(matrix[i][j], matrix[i][k] +
matrix[k][j])
```

## Complexity Analysis 

- Time Complexity: O(V3) due to three nested loops over the vertices. 

- Space Complexity: O(V2) to store the 2D distance adjacency matrix. 

## 4. 0-1 BFS 

## Intuition 

n edge weights are strictly restricted to either `0` or `1` , Dijkstra’s full priority queue becomes unnecessary. We can optimize the traversal by using a double-ended queue ( Deque ) to achieve linear time complexity. 

## Core Theory & Algorithm 

If we traverse an edge with weight `0` , the priority does not change. We push the target node to the front of the deque so it is processed immediately.  

2. If we traverse an edge with weight `1` , we push the target node to the back of 

the deque.  

## Python Implementation 

Python 

```
from collections import deque
INF = float('inf')
def zero_one_bfs(source: int, v: int, adj: list[list[tuple[int, int]]])
-> list[int]:
    dist = [INF] * v
    dist[source] = 0
    dq = deque([source])
    while dq:
        u = dq.popleft()
        for target, weight in adj[u]:
            if dist[u] + weight < dist[target]:
                dist[target] = dist[u] + weight
                if weight == 0:
                    dq.appendleft(target)
                else:
                    dq.append(target)
    return dist
```

## Complexity Analysis 

- Time Complexity: O(V+E). Nodes are inserted and removed from the deque at most once, bypassing the O(logV) priority queue overhead. Space Complexity: O(V). 

## MODULE 2 PRACTICE PROBLEMS 

- Easy: LeetCode 743 - Network Delay Time 

- Medium: LeetCode 1631 - Path With Minimum Effort 

- Hard: LeetCode 787 - Cheapest Flights Within K Stops 

## MODULE 3: DISJOINT SET UNION (DSU) & MINIMUM SPANNING TREES (MST) 

## 1. DISJOINT SET UNION (DSU) 

## Intuition 

DSU manages a collection of non-overlapping sets. It efficiently answers two questions:

1. Are these two elements in the same group? ( `Find` ) 

2. Can we merge these two distinct groups together? ( `Union` ) 

## Real-World Analogy 

Think of a corporate restructuring. Each employee reports to a manager, who reports to a director. If you want to merge two departments, you find the top directors of both departments. One director then agrees to report to the other, merging the entire organizational tree under a single leader. 

## Core Optimizations 

- Path Compression: During a `Find` operation, we update each visited node to point directly to the root. This flattens the tree structure and keeps subsequent lookups fast. 

- Union by Size/Rank: We always attach the smaller tree under the root of the larger tree to keep the overall depth minimal. 

## Implementations 

## C++ Implementation 

C++ 

```
#include <vector>
#include <numeric>
using namespace std;
class DSU {
    vector<int> parent, size;
public:
    DSU(int n) {
        parent.resize(n);
        iota(parent.begin(), parent.end(), 0);
        size.assign(n, 1);
    }
    int find_set(int v) {
        if (v == parent[v]) return v;
        return parent[v] = find_set(parent[v]); // Path compression
    }

    void union_sets(int a, int b) {
        a = find_set(a);
        b = find_set(b);
        if (a != b) {
            if (size[a] < size[b]) swap(a, b);
            parent[b] = a;
            size[a] += size[b];
        }
    }
};
```

## Python Implementation 

Python 

```
class DSU:
    def __init__(self, n: int):
        self.parent = list(range(n))
        self.size = [1] * n
    def find(self, v: int) -> int:
        if v == self.parent[v]:
            return v
        self.parent[v] = self.find(self.parent[v])  # Path compression
        return self.parent[v]
    def union(self, a: int, b: int):
        root_a = self.find(a)
        root_b = self.find(b)
        if root_a != root_b:
            if self.size[root_a] < self.size[root_b]:
                root_a, root_b = root_b, root_a
            self.parent[root_b] = root_a
            self.size[root_a] += self.size[root_b]
```

## Complexity Analysis 

Time Complexity: O(α(V)) per operation, where α is the Inverse Ackermann function. This function grows so slowly that the time complexity is practically O(1) for all realistic inputs. 

Space Complexity: O(V) to maintain parent and size arrays. 


## 2. KRUSKAL’S ALGORITHM (MST CONSTRUCTION) 

## Intuition 

*Minimum Spanning Tree (MST) connects all vertices in a weighted graph using the minimum total edge weight, without introducing cycles. Kruskal's algorithm builds this greedily by adding the cheapest available edges first. 

## Algorithm Steps 

1. Sort all graph edges in non-decreasing order of their weights.  

2. Initialize a DSU structure for V vertices. 

3. Iterate through the sorted edges: if the endpoints of an edge belong to different components, include the edge in the MST and merge the components using the DSU. 


## Implementations 

## C++ Implementation 

C++ 

```
#include <algorithm>
#include <vector>

using namespace std;
struct Edge {
    int u, v, weight;
    bool operator<(const Edge& other) const { return weight <
other.weight; }
};
int kruskalMST(int v, vector<Edge>& edges) {
    sort(edges.begin(), edges.end()); // Edge sorting
    DSU dsu(v);
    int mst_weight = 0;
    int edges_counted = 0;
    for (const auto& edge : edges) {
        if (dsu.find_set(edge.u) != dsu.find_set(edge.v)) {
            dsu.union_sets(edge.u, edge.v);
            mst_weight += edge.weight;
            if (++edges_counted == v - 1) break;
        }
    }
    return mst_weight;
}
```

## Python Implementation 

## Python 

```
def kruskal_mst(v: int, edges: list[tuple[int, int, int]]) -> int:
    # edges format: (weight, u, v)
    edges.sort()
    dsu = DSU(v)
    mst_weight = 0
    edges_included = 0
    for weight, u, val_v in edges:
        if dsu.find(u) != dsu.find(val_v):
            dsu.union(u, val_v)
            mst_weight += weight

            edges_included += 1
            if edges_included == v - 1:
                break
    return mst_weight
```

## Complexity Analysis 

⋅ Time Complexity: O(ElogE+E α(V)). Sorting the edges takes O(ElogE) time, while the subsequent DSU lookups are near-linear. 

- Space Complexity: O(V+E). 

## MODULE 3 PRACTICE PROBLEMS 

- Easy: LeetCode 547 - Number of Provinces 

- Medium: LeetCode 1584 - Min Cost to Connect All Points 

- Hard: LeetCode 803 - Bricks Falling When Hit  

## MODULE 4: GRAPH CONNECTIVITY (SCC, BRIDGES, ARTICULATION POINTS) 

## 1. STRONGLY CONNECTED COMPONENTS (KOSARAJU’S ALGORITHM) 

## Intuition 

*Strongly Connected Component (SCC) is a maximal subgraph within a directed graph where every vertex is reachable from any other vertex in that same subgraph. Kosaraju’s algorithm uses two consecutive DFS passes to isolate these components. 

## Core Theory & Algorithm 

 If an SCC A can reach SCC B via a directed edge, reversing all edges in the graph makes it impossible to travel from A to B. This allows us to cleanly isolate the individual components. 

1. Perform a standard DFS across the entire graph. As each node finishes processing, push it onto a global tracking stack (similar to a topological sort).  

2. Create the Transpose Graph ($G^T$) by reversing the direction of every edge in the graph. 

3. Pop nodes from the stack one by one. If a node is unvisited in $G^T$, launch a second DFS from it. The set of nodes visited during this traversal forms an isolated SCC. 

## Implementations 

## C++ Implementation 

C++ 

```
#include <iostream>
#include <vector>
#include <stack>

using namespace std;

void dfs_finish_time(int u, const vector<vector<int>>&adj,
vector<bool>& visited, stack<int>& st) {

    visited[u] = true;
    for (int neighbor : adj[u]) {
        if (!visited[neighbor]) dfs_finish_time(neighbor, adj, visited,st);
    }
    st.push(u);
}

void dfs_transpose(int u, const vector<vector<int>>& adj_T,
vector<bool>& visited, vector<int>& component) {
    visited[u] = true;
    component.push_back(u);
    for (int neighbor : adj_T[u]) {
        if (!visited[neighbor]) dfs_transpose(neighbor, adj_T, visited,
component);
    }
}

vector<vector<int>> kosarajuSCC(int v, const vector<vector<int>>& adj) {
    stack<int> st;
    vector<bool> visited(v, false);
    for (int i = 0; i < v; i++) {
        if (!visited[i]) dfs_finish_time(i, adj, visited, st);
    }

    vector<vector<int>> adj_T(v);
    for (int u = 0; u < v; u++) {
        for (int neighbor : adj[u]) adj_T[neighbor].push_back(u);
    }

    fill(visited.begin(), visited.end(), false);
    vector<vector<int>> scc_list;

    while (!st.empty()) {
        int u = st.top();
        st.pop();

        if (!visited[u]) {
            vector<int> component;
            dfs_transpose(u, adj_T, visited, component);
            scc_list.push_back(component);

        }
    }
    return scc_list;
}
```

## Python Implementation 

## Python 

```
def kosaraju_scc(v: int, adj: list[list[int]]) -> list[list[int]]:
    stack = []
    visited = [False] * v
    def fill_order(node: int):
        visited[node] = True
        for neighbor in adj[node]:
            if not visited[neighbor]:
                fill_order(neighbor)
        stack.append(node)

    for i in range(v):
        if not visited[i]:
            fill_order(i)

    # Transpose matrix creation
    adj_t = [[] for _ in range(v)]
    for u in range(v):
        for neighbor in adj[u]:
            adj_t[neighbor].append(u)

    visited = [False] * v

    def dfs_collect(node: int, component: list[int]):
        visited[node] = True
        component.append(node)
        for neighbor in adj_t[node]:
            if not visited[neighbor]:
                dfs_collect(neighbor, component)
        
    while stack:   
        u = stack.pop()
        if not visited[u]:
            curr_scc = []
            dfs_collect(u, curr_scc)
            scc_list.append(curr_scc)

    return scc_list
```

## Complexity Analysis 

- Time Complexity: O(V+E). We run two complete traversals of the vertices and edges (one on the original graph and one on its transpose). 

- Space Complexity: O(V+E) to store the reversed graph structure and tracking structures. 

# 2. BRIDGES & ARTICULATION POINTS (TARJAN'S APPROACH) 

## Intuition 

- Bridge: An edge in an undirected graph whose removal breaks the graph into multiple disconnected pieces. 

- Articulation Point: A vertex whose removal breaks the graph into multiple disconnected pieces. 

## Core Theory & Low-Link Tracking 

We run a single DFS pass and track two time metrics for each node u: 

1. `disc[u]` : Discovery time—the sequence step index when the node was first found. 

2. `low[u]` : Low-link value—the lowest discovery time reachable from u using at most one back-edge. 

An edge u→v is a Bridge if and only if: 

       low[v]>disc[u] 

This condition means there is no way to bypass edge u→v to reach node u or any of its ancestors. 

## C++ Bridges Implementation 

C++ 

```
#include <vector>
#include <algorithm>
using namespace std;

void findBridges(int u, int parent, int& timer, const
vector<vector<int>>& adj,
                 vector<int>& disc, vector<int>& low,
vector<vector<int>>& bridges) {
    disc[u] = low[u] = ++timer;
    for (int v : adj[u]) {
        if (v == parent) continue;
        if (disc[v]) { // Back-edge check
            low[u] = min(low[u], disc[v]);
        } else {
            findBridges(v, u, timer, adj, disc, low, bridges);
            low[u] = min(low[u], low[v]);
            if (low[v] > disc[u]) {
                bridges.push_back({u, v}); // Bridge detected
            }
        }

    }
}
```

## Complexity Analysis 

- Time Complexity: O(V+E) using a single standard DFS pass. 

- Space Complexity: O(V). 

## MODULE 4 PRACTICE PROBLEMS 

- Easy: Codeforces 118A - String Task

- Medium: LeetCode 1192 - Critical Connections in a Network

- Hard: HackerRank - Story of a Tree 

## MODULE 5: ADVANCED GRAPH THEORY & NETWORK FLOW 

## 1. HIERHOLZER’S ALGORITHM (EULERIAN PATHS & CIRCUITS) 

## Intuition 

Eulerian Circuit is a path that visits every edge in a graph exactly once and returns to the starting vertex. An Eulerian Path visits every edge exactly once but can start and end at different vertices. Hierholzer’s algorithm builds this path by tracking and merging sub-cycles. 


## Core Conditions 

- Eulerian Circuit: Every vertex must have an equal in-degree and out-degree (for directed graphs), or an even degree (for undirected graphs). 

- Eulerian Path: At most two vertices can have a degree mismatch where ∣ out- 

- degree−in-degree ∣ [cites tart]=1. 

## Python Implementation 

Python 

```
def find_eulerian_circuit(v: int, adj: list[list[int]]) -> list[int]:
    # Destructive execution modifies active edge allocations
    adj_copy = [list(neighbors) for neighbors in adj]
    stack = [0]
    circuit = []
    while stack:
        curr = stack[-1]
        if adj_copy[curr]:
            next_node = adj_copy[curr].pop()
            stack.append(next_node)
        else:
            circuit.append(stack.pop())
    return circuit[::-1]
```

## Complexity Analysis 

- Time Complexity: O(V+E) because we process each edge exactly once. 

- Space Complexity: O(V+E). 

## 2. NETWORK FLOW INTRODUCTION (FORD-FULKERSON & EDMONDS-KARP) 

## Intuition 

Think of a network flow graph as a network of water pipes. Each pipe has a maximum capacity. Our goal is to determine the maximum volume of water we can send from a source node to a sink node. 

## Core Concepts 

- Residual Graph: An internal map showing the remaining capacity on each edge, along with virtual "reverse edges" that allow flows to be redirected or undone. 

- Augmenting Path: A path from source to sink in the residual graph that can accommodate more flow. 

- Edmonds-Karp: A specific implementation of the Ford-Fulkerson method that uses BFS to find the shortest augmenting paths, guaranteeing termination in polynomial time. 

## C++ Edmonds-Karp Implementation 

C++ 

```
#include <iostream>
#include <vector>
#include <queue>
#include <algorithm>

using namespace std;

const int INF = 1e9;

int bfs_augment(int source, int sink, const vector<vector<int>>&
capacity, vector<vector<int>>& residual, vector<int>parent) {
    fill(parent.begin(), parent.end(), -1);
    parent[source] = source;
    queue<pair<int, int>> q;
    q.push({source, INF});
    while (!q.empty()) {
        auto [u, flow] = q.front();
        q.pop();
        for (int v = 0; v < residual.size(); v++) {
            if (parent[v] == -1 && residual[u][v] > 0) {
                parent[v] = u;
                int new_flow = min(flow, residual[u][v]);
                if (v == sink) return new_flow;
                q.push({v, new_flow});
            }
        }
    }
    return 0;
}

int edmondsKarp(int source, int sink, const vector<vector<int>>&
capacity) {
    int v = capacity.size();
    vector<vector<int>> residual = capacity; // Setup initial residual limits
    vector<int> parent(v);
    int max_flow = 0;
    while (true) {
        int flow = bfs_augment(source, sink, capacity, residual,parent);
        if (flow == 0) break;
        max_flow += flow;

        int curr = sink;
        while (curr != source) {
            int prev = parent[curr];
            residual[prev][curr] -= flow;
            residual[curr][prev] += flow; // Enable path redirection
            curr = prev;
        }
    }
    return max_flow;
}
```

## Complexity Analysis 

- Time Complexity: $O(V \cdot E^2)$ for Edmonds-Karp. Using BFS limits the number of ⋅ 

- path augmentations to O(V.E)

- Space Complexity: $O(V^2)$ to store the capacity matrices. 

## MODULE 5 PRACTICE PROBLEMS 

- Easy: LeetCode 797 - All Paths From Source to Target. 

- Medium: LeetCode 332 - Reconstruct Itinerary 

## Extra References Material

1. Douglas B. West, Introduction to Graph Theory, 2nd Edition.
2. Antti Laaksonen, Competitive Programmer’s Handbook.
3. CP-Algorithms Website: https://cp-algorithms.com