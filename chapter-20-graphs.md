# Chapter 20: Graphs

**What You'll Learn**: What graphs are, WHY they exist, graph terminology, representations, BFS, DFS, and solving real-world network problems.

## Table of Contents

1. [Introduction: The Relationship Problem](#introduction-the-relationship-problem)
2. [1. What is a Graph?](#1-what-is-a-graph)
3. [2. Graph Terminology](#2-graph-terminology)
4. [3. Graph Representation](#3-graph-representation)
5. [4. Creating a Graph in C++](#4-creating-a-graph-in-c)
6. [5. Breadth-First Search (BFS)](#5-breadth-first-search-bfs)
7. [6. BFS Applications](#6-bfs-applications)
8. [7. Depth-First Search (DFS)](#7-depth-first-search-dfs)
9. [8. DFS Applications](#8-dfs-applications)
10. [9. BFS vs DFS Comparison](#9-bfs-vs-dfs-comparison)
11. [10. Dijkstra's Shortest Path (Weighted)](#10-dijkstras-shortest-path-weighted)
12. [11. Common Graph Problems](#11-common-graph-problems)
13. [Common Mistakes](#common-mistakes)
14. [Practice Problems](#practice-problems)
15. [Key Takeaways](#key-takeaways)
16. [Graph Algorithms Summary](#graph-algorithms-summary)
17. [What's Next?](#whats-next)

---

## Introduction: The Relationship Problem

Arrays, stacks, and trees are great for **linear** or **hierarchical** relationships. But what about:

**Problem 1: Social Network**
- Alice is friends with Bob and Charlie
- Bob is friends with Alice, Charlie, and David
- Charlie is friends with everyone
- How do you represent this? **Not a tree** (no hierarchy!)

**Problem 2: City Map**
- City A connects to B, C, D
- City B connects to A, E
- Find shortest route from A to E
- How do you represent roads? **Not a list!**

**Problem 3: Course Prerequisites**
- CS101 required for CS201, CS202
- CS201 required for CS301
- Can you take CS301 first? **Need to track dependencies!**

**Solution: Graphs** - the most flexible data structure!

---

## 1. What is a Graph?

A **graph** is a collection of **vertices** (nodes) connected by **edges** (links).

**Formal definition**:
- G = (V, E)
- V = set of vertices
- E = set of edges (pairs of vertices)

**Visual Example**:
```
     A --- B
     |     |
     |     |
     C --- D

Vertices: {A, B, C, D}
Edges: {(A,B), (A,C), (B,D), (C,D)}
```

**Think of it like**:
- **Cities and roads**: Cities = vertices, roads = edges
- **People and friendships**: People = vertices, friendships = edges
- **Web pages and links**: Pages = vertices, hyperlinks = edges

---

## 2. Graph Terminology

### Vertex (Node)
The fundamental unit (dots in the diagram).

### Edge (Link)
Connection between two vertices (lines in the diagram).

### Directed vs Undirected

**Undirected**: Edges have no direction (two-way street)
```
A --- B    (A connects to B, B connects to A)
```

**Directed**: Edges have direction (one-way street)
```
A --> B    (A points to B, but B doesn't point to A)
```

---

### Weighted vs Unweighted

**Unweighted**: All edges equal
```
A --- B
```

**Weighted**: Edges have values (distance, cost, time)
```
A --5-- B    (Distance = 5)
```

---

### Degree

**Degree of vertex** = number of edges connected to it.

```
     A --- B
     |     |
     C --- D

Degree(A) = 2 (connects to B and C)
Degree(B) = 2
Degree(C) = 2
Degree(D) = 2
```

For **directed graphs**:
- **In-degree**: Edges coming in
- **Out-degree**: Edges going out

```
A --> B --> C
      ↓
      D

In-degree(B) = 1 (from A)
Out-degree(B) = 2 (to C and D)
```

---

### Path

**Path** = sequence of vertices connected by edges.

```
A --- B --- C --- D

Path from A to D: A → B → C → D (length = 3)
```

---

### Cycle

**Cycle** = path that starts and ends at same vertex.

```
A --- B
|     |
C --- D

Cycle: A → B → D → C → A
```

---

### Connected Graph

**Connected** = path exists between every pair of vertices.

**Connected** ✓:
```
A --- B
|     |
C --- D
```

**Not connected** ✗:
```
A --- B    C --- D
(Two separate components)
```

---

## 3. Graph Representation

Two main ways to store graphs in memory:

### Adjacency Matrix

**2D array** where matrix[i][j] = 1 if edge exists, 0 otherwise.

**Example**:
```
Graph:
     0 --- 1
     |     |
     2 --- 3

Adjacency Matrix:
    0  1  2  3
0 [ 0  1  1  0 ]
1 [ 1  0  0  1 ]
2 [ 1  0  0  1 ]
3 [ 0  1  1  0 ]

matrix[0][1] = 1 means edge 0-1 exists
matrix[0][3] = 0 means no edge 0-3
```

**For weighted graphs**:
```
Graph:
     0 --5-- 1
     |       |
    10       3
     |       |
     2 --7-- 3

Matrix:
    0   1   2   3
0 [ 0   5  10   0 ]
1 [ 5   0   0   3 ]
2 [10   0   0   7 ]
3 [ 0   3   7   0 ]
```

**Code**:
```cpp
// Unweighted graph
vector<vector<int>> graph(n, vector<int>(n, 0));

// Add edge
graph[u][v] = 1;
graph[v][u] = 1;  // For undirected

// Check edge
if (graph[u][v] == 1) {
    cout << "Edge exists";
}
```

**Pros**:
- ✅ Fast edge lookup: O(1)
- ✅ Simple to implement

**Cons**:
- ❌ Space: O(V²) even for sparse graphs
- ❌ Iterating over neighbors: O(V)

---

### Adjacency List

**Array of lists** where list[i] contains neighbors of vertex i.

**Example**:
```
Graph:
     0 --- 1
     |     |
     2 --- 3

Adjacency List:
0 → [1, 2]
1 → [0, 3]
2 → [0, 3]
3 → [1, 2]
```

**For weighted graphs**:
```
Graph:
     0 --5-- 1
     |       |
    10       3
     |       |
     2 --7-- 3

Adjacency List:
0 → [(1,5), (2,10)]
1 → [(0,5), (3,3)]
2 → [(0,10), (3,7)]
3 → [(1,3), (2,7)]
```

**Code**:
```cpp
// Unweighted
vector<vector<int>> graph(n);

// Add edge
graph[u].push_back(v);
graph[v].push_back(u);  // For undirected

// Iterate neighbors
for (int neighbor : graph[u]) {
    cout << neighbor << " ";
}

// Weighted (using pairs)
vector<vector<pair<int, int>>> graph(n);  // {neighbor, weight}

// Add weighted edge
graph[u].push_back({v, weight});
graph[v].push_back({u, weight});
```

**Pros**:
- ✅ Space efficient: O(V + E)
- ✅ Fast neighbor iteration

**Cons**:
- ❌ Edge lookup: O(degree)

---

## 4. Creating a Graph in C++

### Unweighted Adjacency List

```cpp
#include <iostream>
#include <vector>
using namespace std;

class Graph {
    int V;  // Number of vertices
    vector<vector<int>> adj;  // Adjacency list

public:
    Graph(int vertices) : V(vertices), adj(vertices) {}

    void addEdge(int u, int v) {
        adj[u].push_back(v);
        adj[v].push_back(u);  // For undirected graph
    }

    void printGraph() {
        for (int i = 0; i < V; i++) {
            cout << i << " -> ";
            for (int neighbor : adj[i]) {
                cout << neighbor << " ";
            }
            cout << endl;
        }
    }

    vector<int>& getNeighbors(int u) {
        return adj[u];
    }
};

int main() {
    Graph g(4);

    g.addEdge(0, 1);
    g.addEdge(0, 2);
    g.addEdge(1, 3);
    g.addEdge(2, 3);

    g.printGraph();
    // Output:
    // 0 -> 1 2
    // 1 -> 0 3
    // 2 -> 0 3
    // 3 -> 1 2

    return 0;
}
```

---

### Weighted Adjacency List

```cpp
#include <iostream>
#include <vector>
using namespace std;

class WeightedGraph {
    int V;
    vector<vector<pair<int, int>>> adj;  // {neighbor, weight}

public:
    WeightedGraph(int vertices) : V(vertices), adj(vertices) {}

    void addEdge(int u, int v, int weight) {
        adj[u].push_back({v, weight});
        adj[v].push_back({u, weight});  // For undirected
    }

    void printGraph() {
        for (int i = 0; i < V; i++) {
            cout << i << " -> ";
            for (auto [neighbor, weight] : adj[i]) {
                cout << "(" << neighbor << "," << weight << ") ";
            }
            cout << endl;
        }
    }
};

int main() {
    WeightedGraph g(4);

    g.addEdge(0, 1, 5);
    g.addEdge(0, 2, 10);
    g.addEdge(1, 3, 3);
    g.addEdge(2, 3, 7);

    g.printGraph();
    // Output:
    // 0 -> (1,5) (2,10)
    // 1 -> (0,5) (3,3)
    // 2 -> (0,10) (3,7)
    // 3 -> (1,3) (2,7)

    return 0;
}
```

---

## 5. Breadth-First Search (BFS)

**BFS** explores graph **level by level** (like ripples in water).

**Algorithm**:
1. Start at source vertex
2. Visit all neighbors (level 1)
3. Visit all neighbors of neighbors (level 2)
4. Continue until all reachable vertices visited

**Uses queue**: FIFO (First In, First Out)

**Visual**:
```
Start at 0:

     0         Level 0
    / \
   1   2       Level 1
   |   |
   3   4       Level 2

BFS Order: 0, 1, 2, 3, 4
```

**Implementation**:
```cpp
#include <iostream>
#include <vector>
#include <queue>
using namespace std;

void BFS(vector<vector<int>>& graph, int start) {
    int n = graph.size();
    vector<bool> visited(n, false);
    queue<int> q;

    // Start from source
    visited[start] = true;
    q.push(start);

    cout << "BFS traversal: ";

    while (!q.empty()) {
        int u = q.front();
        q.pop();

        cout << u << " ";

        // Visit all neighbors
        for (int neighbor : graph[u]) {
            if (!visited[neighbor]) {
                visited[neighbor] = true;
                q.push(neighbor);
            }
        }
    }

    cout << endl;
}

int main() {
    // Graph:
    //     0 --- 1
    //     |     |
    //     2 --- 3

    vector<vector<int>> graph(4);
    graph[0] = {1, 2};
    graph[1] = {0, 3};
    graph[2] = {0, 3};
    graph[3] = {1, 2};

    BFS(graph, 0);
    // Output: 0 1 2 3

    return 0;
}
```

**Time Complexity**: O(V + E)
**Space Complexity**: O(V)

---

## 6. BFS Applications

### Application 1: Shortest Path (Unweighted)

```cpp
#include <iostream>
#include <vector>
#include <queue>
using namespace std;

vector<int> shortestPath(vector<vector<int>>& graph, int start, int end) {
    int n = graph.size();
    vector<bool> visited(n, false);
    vector<int> parent(n, -1);
    queue<int> q;

    visited[start] = true;
    q.push(start);

    while (!q.empty()) {
        int u = q.front();
        q.pop();

        if (u == end) break;

        for (int v : graph[u]) {
            if (!visited[v]) {
                visited[v] = true;
                parent[v] = u;
                q.push(v);
            }
        }
    }

    // Reconstruct path
    vector<int> path;
    int current = end;

    if (parent[current] == -1 && current != start) {
        return {};  // No path exists
    }

    while (current != -1) {
        path.push_back(current);
        current = parent[current];
    }

    reverse(path.begin(), path.end());
    return path;
}

int main() {
    vector<vector<int>> graph(5);
    graph[0] = {1, 2};
    graph[1] = {0, 3};
    graph[2] = {0, 4};
    graph[3] = {1, 4};
    graph[4] = {2, 3};

    vector<int> path = shortestPath(graph, 0, 4);

    cout << "Shortest path from 0 to 4: ";
    for (int v : path) {
        cout << v << " ";
    }
    // Output: 0 2 4

    return 0;
}
```

---

### Application 2: Check if Graph is Bipartite

```cpp
#include <iostream>
#include <vector>
#include <queue>
using namespace std;

bool isBipartite(vector<vector<int>>& graph) {
    int n = graph.size();
    vector<int> color(n, -1);  // -1 = uncolored, 0 and 1 = two colors

    for (int start = 0; start < n; start++) {
        if (color[start] == -1) {
            queue<int> q;
            q.push(start);
            color[start] = 0;

            while (!q.empty()) {
                int u = q.front();
                q.pop();

                for (int v : graph[u]) {
                    if (color[v] == -1) {
                        color[v] = 1 - color[u];  // Opposite color
                        q.push(v);
                    } else if (color[v] == color[u]) {
                        return false;  // Same color as neighbor!
                    }
                }
            }
        }
    }

    return true;
}

int main() {
    // Bipartite graph
    vector<vector<int>> g1(4);
    g1[0] = {1, 3};
    g1[1] = {0, 2};
    g1[2] = {1, 3};
    g1[3] = {0, 2};

    cout << "Graph 1 is bipartite: " << isBipartite(g1) << endl;  // 1 (true)

    // Not bipartite (triangle)
    vector<vector<int>> g2(3);
    g2[0] = {1, 2};
    g2[1] = {0, 2};
    g2[2] = {0, 1};

    cout << "Graph 2 is bipartite: " << isBipartite(g2) << endl;  // 0 (false)

    return 0;
}
```

---

## 7. Depth-First Search (DFS)

**DFS** explores graph **as deep as possible** before backtracking.

**Algorithm**:
1. Start at source vertex
2. Go as deep as possible along one path
3. Backtrack when stuck
4. Try next unexplored path

**Uses stack** (or recursion): LIFO (Last In, First Out)

**Visual**:
```
     0
    / \
   1   2
  / \
 3   4

DFS Order: 0, 1, 3, 4, 2
(Go deep: 0→1→3, backtrack, go to 4, backtrack, go to 2)
```

**Recursive Implementation**:
```cpp
#include <iostream>
#include <vector>
using namespace std;

void DFSHelper(vector<vector<int>>& graph, int u, vector<bool>& visited) {
    visited[u] = true;
    cout << u << " ";

    for (int v : graph[u]) {
        if (!visited[v]) {
            DFSHelper(graph, v, visited);
        }
    }
}

void DFS(vector<vector<int>>& graph, int start) {
    int n = graph.size();
    vector<bool> visited(n, false);

    cout << "DFS traversal: ";
    DFSHelper(graph, start, visited);
    cout << endl;
}

int main() {
    vector<vector<int>> graph(5);
    graph[0] = {1, 2};
    graph[1] = {0, 3, 4};
    graph[2] = {0};
    graph[3] = {1};
    graph[4] = {1};

    DFS(graph, 0);
    // Output: 0 1 3 4 2

    return 0;
}
```

---

**Iterative Implementation (using stack)**:
```cpp
#include <iostream>
#include <vector>
#include <stack>
using namespace std;

void DFS_Iterative(vector<vector<int>>& graph, int start) {
    int n = graph.size();
    vector<bool> visited(n, false);
    stack<int> s;

    s.push(start);

    cout << "DFS traversal: ";

    while (!s.empty()) {
        int u = s.top();
        s.pop();

        if (!visited[u]) {
            visited[u] = true;
            cout << u << " ";

            // Push neighbors in reverse to match recursive order
            for (int i = graph[u].size() - 1; i >= 0; i--) {
                if (!visited[graph[u][i]]) {
                    s.push(graph[u][i]);
                }
            }
        }
    }

    cout << endl;
}
```

**Time Complexity**: O(V + E)
**Space Complexity**: O(V)

---

## 8. DFS Applications

### Application 1: Detect Cycle

```cpp
#include <iostream>
#include <vector>
using namespace std;

bool hasCycleHelper(vector<vector<int>>& graph, int u, vector<bool>& visited, int parent) {
    visited[u] = true;

    for (int v : graph[u]) {
        if (!visited[v]) {
            if (hasCycleHelper(graph, v, visited, u)) {
                return true;
            }
        } else if (v != parent) {
            return true;  // Found back edge (cycle)
        }
    }

    return false;
}

bool hasCycle(vector<vector<int>>& graph) {
    int n = graph.size();
    vector<bool> visited(n, false);

    for (int i = 0; i < n; i++) {
        if (!visited[i]) {
            if (hasCycleHelper(graph, i, visited, -1)) {
                return true;
            }
        }
    }

    return false;
}

int main() {
    // Graph with cycle
    vector<vector<int>> g1(3);
    g1[0] = {1, 2};
    g1[1] = {0, 2};
    g1[2] = {0, 1};

    cout << "Graph has cycle: " << hasCycle(g1) << endl;  // 1 (true)

    // Tree (no cycle)
    vector<vector<int>> g2(3);
    g2[0] = {1, 2};
    g2[1] = {0};
    g2[2] = {0};

    cout << "Tree has cycle: " << hasCycle(g2) << endl;  // 0 (false)

    return 0;
}
```

---

### Application 2: Connected Components

```cpp
#include <iostream>
#include <vector>
using namespace std;

void DFSComponent(vector<vector<int>>& graph, int u, vector<bool>& visited) {
    visited[u] = true;

    for (int v : graph[u]) {
        if (!visited[v]) {
            DFSComponent(graph, v, visited);
        }
    }
}

int countComponents(vector<vector<int>>& graph) {
    int n = graph.size();
    vector<bool> visited(n, false);
    int count = 0;

    for (int i = 0; i < n; i++) {
        if (!visited[i]) {
            DFSComponent(graph, i, visited);
            count++;
        }
    }

    return count;
}

int main() {
    // Two separate components
    vector<vector<int>> graph(5);
    graph[0] = {1};
    graph[1] = {0, 2};
    graph[2] = {1};
    graph[3] = {4};
    graph[4] = {3};

    cout << "Number of components: " << countComponents(graph) << endl;
    // Output: 2

    return 0;
}
```

---

### Application 3: Topological Sort (DAG)

**Topological sort** = linear ordering of vertices for directed acyclic graph.

**Use case**: Course prerequisites, task scheduling

```cpp
#include <iostream>
#include <vector>
#include <stack>
using namespace std;

void topologicalSortHelper(vector<vector<int>>& graph, int u,
                          vector<bool>& visited, stack<int>& s) {
    visited[u] = true;

    for (int v : graph[u]) {
        if (!visited[v]) {
            topologicalSortHelper(graph, v, visited, s);
        }
    }

    s.push(u);  // Add to stack after visiting all neighbors
}

vector<int> topologicalSort(vector<vector<int>>& graph) {
    int n = graph.size();
    vector<bool> visited(n, false);
    stack<int> s;

    for (int i = 0; i < n; i++) {
        if (!visited[i]) {
            topologicalSortHelper(graph, i, visited, s);
        }
    }

    vector<int> result;
    while (!s.empty()) {
        result.push_back(s.top());
        s.pop();
    }

    return result;
}

int main() {
    // Directed graph (course prerequisites)
    // 0: CS101, 1: CS201, 2: CS202, 3: CS301
    vector<vector<int>> graph(4);
    graph[0] = {1, 2};  // CS101 → CS201, CS202
    graph[1] = {3};     // CS201 → CS301
    graph[2] = {3};     // CS202 → CS301

    vector<int> order = topologicalSort(graph);

    cout << "Course order: ";
    for (int course : order) {
        cout << "CS" << (course + 1) << "01 ";
    }
    // Output: CS101 CS202 CS201 CS301 (or CS101 CS201 CS202 CS301)

    return 0;
}
```

---

## 9. BFS vs DFS Comparison

| Feature | BFS | DFS |
|---------|-----|-----|
| **Data Structure** | Queue | Stack (or recursion) |
| **Exploration** | Level by level | As deep as possible |
| **Use Cases** | Shortest path, level order | Cycle detection, topological sort |
| **Space** | O(V) - stores entire level | O(V) - recursion depth |
| **Finds shortest path** | Yes (unweighted) | No |
| **Memory** | More (queue larger) | Less (stack smaller) |

---

## 10. Dijkstra's Shortest Path (Weighted)

**Dijkstra's algorithm** finds shortest path in **weighted graphs**.

**Algorithm**:
1. Start with distance 0 to source, infinity to others
2. Pick unvisited vertex with minimum distance
3. Update distances to neighbors
4. Repeat until all visited

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <limits>
using namespace std;

const int INF = numeric_limits<int>::max();

vector<int> dijkstra(vector<vector<pair<int, int>>>& graph, int start) {
    int n = graph.size();
    vector<int> dist(n, INF);
    priority_queue<pair<int, int>,
                   vector<pair<int, int>>,
                   greater<pair<int, int>>> pq;  // {distance, vertex}

    dist[start] = 0;
    pq.push({0, start});

    while (!pq.empty()) {
        auto [d, u] = pq.top();
        pq.pop();

        if (d > dist[u]) continue;  // Already processed

        for (auto [v, weight] : graph[u]) {
            int newDist = dist[u] + weight;

            if (newDist < dist[v]) {
                dist[v] = newDist;
                pq.push({newDist, v});
            }
        }
    }

    return dist;
}

int main() {
    // Weighted graph
    //      0 --1-- 1
    //      |       |
    //      4       2
    //      |       |
    //      2 --1-- 3

    vector<vector<pair<int, int>>> graph(4);
    graph[0] = {{1, 1}, {2, 4}};
    graph[1] = {{0, 1}, {3, 2}};
    graph[2] = {{0, 4}, {3, 1}};
    graph[3] = {{1, 2}, {2, 1}};

    vector<int> distances = dijkstra(graph, 0);

    cout << "Shortest distances from vertex 0:" << endl;
    for (int i = 0; i < distances.size(); i++) {
        cout << "To " << i << ": " << distances[i] << endl;
    }
    // Output:
    // To 0: 0
    // To 1: 1
    // To 2: 4
    // To 3: 3

    return 0;
}
```

**Time Complexity**: O((V + E) log V) with priority queue
**Space Complexity**: O(V)

---

## 11. Common Graph Problems

### Problem 1: Number of Islands

Count connected components in 2D grid.

```cpp
void dfs(vector<vector<char>>& grid, int i, int j) {
    if (i < 0 || i >= grid.size() || j < 0 || j >= grid[0].size() ||
        grid[i][j] == '0') {
        return;
    }

    grid[i][j] = '0';  // Mark visited

    dfs(grid, i+1, j);
    dfs(grid, i-1, j);
    dfs(grid, i, j+1);
    dfs(grid, i, j-1);
}

int numIslands(vector<vector<char>>& grid) {
    int count = 0;

    for (int i = 0; i < grid.size(); i++) {
        for (int j = 0; j < grid[0].size(); j++) {
            if (grid[i][j] == '1') {
                dfs(grid, i, j);
                count++;
            }
        }
    }

    return count;
}
```

---

### Problem 2: Clone Graph

Deep copy of graph.

```cpp
struct Node {
    int val;
    vector<Node*> neighbors;

    Node(int v) : val(v) {}
};

Node* cloneGraph(Node* node, unordered_map<Node*, Node*>& visited) {
    if (!node) return nullptr;
    if (visited.count(node)) return visited[node];

    Node* clone = new Node(node->val);
    visited[node] = clone;

    for (Node* neighbor : node->neighbors) {
        clone->neighbors.push_back(cloneGraph(neighbor, visited));
    }

    return clone;
}
```

---

## Common Mistakes

### Mistake 1: Forgetting to Mark Visited

```cpp
void DFS(vector<vector<int>>& graph, int u) {
    // FORGOT: visited[u] = true;

    for (int v : graph[u]) {
        DFS(graph, v);  // Infinite loop if cycle exists!
    }
}

// FIX: Always mark visited
visited[u] = true;
```

---

### Mistake 2: Wrong Direction in Directed Graph

```cpp
// Adding edge u -> v
graph[u].push_back(v);
graph[v].push_back(u);  // WRONG for directed graph!

// FIX: Only add one direction
graph[u].push_back(v);
```

---

### Mistake 3: Not Checking Disconnected Components

```cpp
// Only starts from vertex 0
BFS(graph, 0);  // Misses disconnected parts!

// FIX: Check all vertices
for (int i = 0; i < n; i++) {
    if (!visited[i]) {
        BFS(graph, i);
    }
}
```

---

## Practice Problems

### Problem 1: Course Schedule
Can you finish all courses given prerequisites? (Cycle detection)

### Problem 2: Word Ladder
Find shortest transformation from one word to another.

### Problem 3: Network Delay Time
Minimum time for signal to reach all nodes.

### Problem 4: All Paths from Source to Target
Find all possible paths in DAG.

### Problem 5: Minimum Spanning Tree
Find minimum cost to connect all vertices (Prim's/Kruskal's).

---

## Key Takeaways

✅ **Graph** = vertices connected by edges
✅ **Directed**: One-way edges, **Undirected**: Two-way edges
✅ **Weighted**: Edges have values (distance, cost)
✅ **Adjacency matrix**: O(1) edge lookup, O(V²) space
✅ **Adjacency list**: O(V + E) space, efficient for sparse graphs
✅ **BFS**: Level-by-level (queue), finds shortest path
✅ **DFS**: Depth-first (stack/recursion), detects cycles
✅ **Dijkstra**: Shortest path in weighted graphs
✅ **Topological sort**: Order tasks with dependencies
✅ **Applications**: Social networks, maps, web, scheduling

---

## Graph Algorithms Summary

| Algorithm | Use Case | Time Complexity |
|-----------|----------|-----------------|
| **BFS** | Shortest path (unweighted) | O(V + E) |
| **DFS** | Cycle detection, topological sort | O(V + E) |
| **Dijkstra** | Shortest path (weighted, positive) | O((V+E) log V) |
| **Topological Sort** | Task scheduling | O(V + E) |
| **Connected Components** | Find clusters | O(V + E) |

---

## What's Next?

Congratulations! You've completed the C++ & Data Structures course! You now have the foundation to:
- Build efficient algorithms
- Solve complex coding problems
- Ace technical interviews
- Design scalable systems

**Continue learning**:
- Advanced algorithms (dynamic programming, greedy)
- System design
- Competitive programming
- Open source contributions

Keep coding and keep learning!

---

**Chapter Progress**: ✅ Chapters 1-20 Complete! 🎉 Course Finished!
