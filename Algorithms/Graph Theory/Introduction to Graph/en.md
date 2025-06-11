# Introduction to Graph

## Definition

A **graph** $G = (V, E)$ is a collection of **vertices** (nodes) $V$ and **edges** $E$ connecting pairs of vertices. Many contest problems—from modeling social networks of friends to mapping cities and roads, from scheduling tasks with prerequisites to crawling the web—boil down to graph questions.

Before diving into the details, note that graphs fall into categories based on their edges: **directed** vs **undirected**, and **weighted** vs **unweighted**. These distinctions determine which structures (trees, DAGs, cycles) and techniques (BFS, topological sort, union-find, SCC algorithms) apply.

---

## Terminology

### Common Terms

| Term            | Definition                                                                                 |
|-----------------|--------------------------------------------------------------------------------------------|
| **Vertex**      | A node in the graph                                                                        |
| **Edge**        | A connection between two vertices                                                          |
| **Self-loop**   | An edge $(u,u)$ from a vertex back to itself                                              |
| **Multi-edge**  | More than one edge connecting the same pair of vertices                                    |
| **Subgraph**    | A graph formed by selecting a subset of the original vertices and edges                    |
| **Path**        | A sequence of vertices where each consecutive pair is connected by an edge                 |
| **Simple Path** | A path in which no vertex is repeated                                                      |
| **Cycle**       | A path whose first and last vertices are the same                                         |
| **Simple Cycle**| A cycle in which no vertices (except the start/end) are repeated                           |
| **Acyclic**     | A graph with no cycles                                                         |
| **Cyclic**      | A graph that contains at least one cycle                                      |
| **Component**   | A set of vertices in which each pair is connected by some path   

### Undirected-Specific

| Term                    | Definition                                                                     |
|-------------------------|--------------------------------------------------------------------------------|
| **Degree**              | The number of edges incident to a vertex                                       |
| **Connected Component** | A maximal set of vertices where each pair is connected by some path            |
| **Connected Graph**     | An undirected graph with exactly one connected component                       |
| **Tree**                | A connected, acyclic undirected graph with exactly $V - 1$ edges        |

### Directed-Specific

| Term                              | Definition                                                                         |
|-----------------------------------|------------------------------------------------------------------------------------|
| **In-degree**                     | The number of edges entering a vertex                                              |
| **Out-degree**                    | The number of edges leaving a vertex                                               |
| **DAG**                           | Directed Acyclic Graph—no directed cycles                                          |
| **Strongly Connected Component** | A maximal subgraph where each vertex can reach every other via directed paths      |
| **Reachability**                  | The existence of a directed path from one vertex to another                        |

---

---

## Types of Graphs

There are several basic graph types, each defined by edge direction and weight or special structure:

- **Undirected Graph**: Edges have no direction — $(u, v) = (v, u)$.  
- **Directed Graph (Digraph)**: Edges have a direction — $(u, v) \neq (v, u)$.
- **Unweighted Graph**: All edges are considered equal (weight = 1).
- **Weighted Graph**: Each edge has a cost or weight.   
- **Cyclic Graph**: Contains at least one cycle. 
- **Acyclic Graph**: Contains at least one cycle.
- **Tree**: A connected, undirected, acyclic graph with $n$ nodes and $n-1$ edges.  
- **DAG (Directed Acyclic Graph)**: A directed graph with no cycles.
- **Connected Graph**: an undirected graph with exactly one connected component.  
- **Disconnected Graph**: an undirected graph with two or more separate components.

---

## Tree Properties


A **tree** is a special undirected graph with no cycles and exactly $n-1$ edges on $n$ vertices. We often choose one node as the *root* to discuss:

- **Depth** of a vertex is the number of edges from the root down to that vertex.  
- **Height** of the tree is the maximum depth among all vertices.  
- **Diameter** is the longest shortest-path between any two vertices in the tree.

---


### Example 1: Weighted Undirected with Self-Loop and Multi-Edge

This is the input graph  
$$
G = (V, E), \quad
V = \{1, 2, 3\}, \quad
E = \{(1,1,5),\, (1,2,3),\, (1,2,3),\, (2,3,4)\}.
$$

<div align="center">
    <img src="images/example1.png" alt="Example 1" width="400">
</div>

This is a **weighted**, **undirected** graph with special edges. It contains a **self-loop** at vertex 1 (edge \((1,1,5)\)) and a **multi-edge** between 1 and 2 (two copies of \((1,2,3)\)). The edge \((2,3,4)\) is a normal undirected connection of weight 4.

---

### Example 2: Path vs Simple Path

This is the input graph  
$$
G = (V, E), \quad
V = \{1,2,3,4,5\}, \quad
E = \{(1,2),\, (2,3),\, (3,4),\, (4,5),\, (2,4)\}.
$$

<div align="center">
    <img src="images/example2.png" alt="Example 2" width="400">
</div>

This is an **unweighted**, **undirected** graph. Consider:

- A **path** from 1 to 5 can be \(1 \rightarrow 2 \rightarrow 3 \rightarrow 4 \rightarrow 5\).  
- A **non-simple path** might revisit a vertex, e.g. \(1 \rightarrow 2 \rightarrow 3 \rightarrow 2 \rightarrow 4 \rightarrow 5\).  
- A **simple path** has no repeats, such as \(1 \rightarrow 2 \rightarrow 4 \rightarrow 5\).

---

### Example 3: Cycle vs Simple Cycle

This is the input graph  
$$
G = (V, E), \quad
V = \{1,2,3,4\}, \quad
E = \{(1,2),\, (2,3),\, (3,1),\, (2,4)\}.
$$

<div align="center">
    <img src="images/example3.png" alt="Example 3" width="400">
</div>

This is an **unweighted**, **undirected** graph. Notice:

- A **cycle** is \(1 \rightarrow 2 \rightarrow 3 \rightarrow 1\).  
- A **non-simple cycle** might be \(1 \rightarrow 2 \rightarrow 3 \rightarrow 2 \rightarrow 1\).  
- A **simple cycle** visits each vertex once before returning, e.g. \(1 \rightarrow 2 \rightarrow 3 \rightarrow 1\).

---

### Example 4: Disconnected Graph and Components

This is the input graph  
$$
G = (V, E), \quad
V = \{1,2,3,4,5,6,7\}, \quad
E = \{(1,2),\, (2,3),\, (4,5),\, (5,6)\}.
$$

<div align="center">
    <img src="images/example4.png" alt="Example 4" width="400">
</div>

This **unweighted**, **undirected** graph is **disconnected** and splits into three components:

1. Component \(\{1,2,3\}\), since 1–2–3 form a chain.  
2. Component \(\{4,5,6\}\), since 4–5–6 form a chain.  
3. Component \(\{7\}\), an isolated vertex.

---

### Example 5: Weighted Undirected Degrees

This is the input graph  
$$
G = (V, E), \quad
V = \{1,2,3,4\}, \quad
E = \{(1,2,2),\, (1,3,5),\, (2,4,1),\, (3,4,3)\}.
$$

<div align="center">
    <img src="images/example5.png" alt="Example 5" width="400">
</div>

This **weighted**, **undirected** graph has degrees:

- **degree(1) = 2** (neighbors: 2, 3)  
- **degree(2) = 2** (neighbors: 1, 4)  
- **degree(3) = 2** (neighbors: 1, 4)  
- **degree(4) = 2** (neighbors: 2, 3)

---

### Example 6: Tree Heights, Depths, Diameter

This is the input tree  
$$
G = (V, E), \quad
V = \{1,2,3,4,5\}, \quad
E = \{(1,2),\, (1,3),\, (3,4),\, (3,5)\}.
$$

<div align="center">
    <img src="images/example6.png" alt="Example 6" width="400">
</div>

This is an **undirected**, **acyclic** graph (a tree). Root at 1:

- **depth(1) = 0**, **depth(2) = 1**, **depth(3) = 1**, **depth(4) = 2**, **depth(5) = 2**  
- **height** = 2 (the greatest depth)  
- **diameter** = 3 (longest path, e.g. 2–1–3–4)

---

### Example 7: DAG In/Out Degrees

This is the input DAG  
$$
G = (V, E), \quad
V = \{1,2,3,4\}, \quad
E = \{(1,2),\, (1,3),\, (2,4),\, (3,4)\}.
$$

<div align="center">
    <img src="images/example7.png" alt="Example 7" width="400">
</div>

This **directed**, **acyclic** graph has:

- **in(1) = 0**, **out(1) = 2**  
- **in(2) = 1**, **out(2) = 1**  
- **in(3) = 1**, **out(3) = 1**  
- **in(4) = 2**, **out(4) = 0**

---

### Example 8: Strongly Connected Components

This is the input graph  
$$
G = (V, E), \quad
V = \{1,2,3,4\}, \quad
E = \{(1,2),\, (2,3),\, (3,1),\, (3,4)\}.
$$

<div align="center">
    <img src="images/example8.png" alt="Example 8" width="400">
</div>

This **directed** graph has SCCs:

- **SCC 1:** \(\{1,2,3\}\) since 1→2→3→1 forms a cycle.  
- **SCC 2:** \(\{4\}\) stands alone without return edges.

---


## Adjacency List

- Stores a list of neighbors for each node  
- Efficient for **sparse graphs**  
- **Space complexity**: $O(n + m)$  
- **Edge check complexity**: $O(\text{degree})$  


=== "c++"
```c++
vector<vector<int>> adj(n);
adj[u].push_back(v);
adj[v].push_back(u);  // if undirected
```

=== "Python"
```python
adj = [[] for _ in range(n)]
adj[u].append(v)
adj[v].append(u)  # if undirected
```

## Adjacency Matrix

- 2D matrix of size $n \times n$  
- $adj[u][v] = 1$ means there is an edge from node $u$ to node $v$  
- Suitable for **dense graphs**  
- **Space complexity**: $O(n^2)$  
- **Edge check complexity**: $O(1)$  
- Simple and fast lookups  
- Not space-efficient for sparse graphs  

=== "c++"
```c++
vector<vector<int>> adj(n, vector<int>(n, 0));
adj[u][v] = 1;
adj[v][u] = 1;  // if undirected
```

=== "Python"
```python
adj = [[0] * n for _ in range(n)]
adj[u][v] = 1
adj[v][u] = 1  # if undirected
```

---

## Graph Traversal

Graph traversal refers to the techniques used to visit all the nodes in a graph, much like iterating through an array or a vector. Two fundamental methods are **Depth-First Search (DFS)**, which uses a stack or recursion to explore as far as possible along each branch, and **Breadth-First Search (BFS)**, which uses a queue to explore neighbors level by level. We will cover these traversal algorithms in detail in upcoming blog posts.
