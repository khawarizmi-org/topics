# Introduction to Graph

## Definition

A **graph** is a way to show how things relate. Each **vertex** (or **node**) represents an item, and each **edge** connects two vertices to show a link. For example, in a social network, people are vertices and friendships are edges; on a map, cities are vertices and roads are edges; in scheduling, tasks are vertices and dependencies are directed edges. Graphs can be **undirected** (links work both ways) or **directed** (links point one way), and **unweighted** (all links equal) or **weighted** (links carry a cost). These choices affect how we study and solve graph problems.

---

## Terminology

### Common Terms

| Term            | Definition                                                                    |
|-----------------|-------------------------------------------------------------------------------|
| **Vertex**      | A point or item in the graph                                                  |
| **Edge**        | A link connecting two vertices                                                |
| **Self-loop**   | An edge that starts and ends at the same vertex                               |
| **Multi-edge**  | Two or more edges connecting the same pair of vertices                        |
| **Subgraph**    | A smaller graph formed by selecting some vertices and edges                   |
| **Path**        | A sequence of vertices where each consecutive pair is connected by an edge    |
| **Simple Path** | A path that does not repeat any vertex                                        |
| **Cycle**       | A path whose first and last vertices are the same                             |
| **Simple Cycle**| A cycle that does not repeat any vertices except the start/end                |
| **Acyclic**     | A graph with no cycles                                                         |
| **Cyclic**      | A graph that contains at least one cycle                                      |
| **Component**   | A set of vertices in which each pair is connected by some path                |

### Undirected-Specific

| Term                    | Definition                                                               |
|-------------------------|--------------------------------------------------------------------------|
| **Degree**              | The number of edges touching a vertex                                    |
| **Connected Component** | A maximal group of vertices where each can reach every other            |
| **Connected Graph**     | An undirected graph that has exactly one connected component             |
| **Tree**                | A connected, acyclic undirected graph with exactly \(|V| - 1\) edges     |

### Directed-Specific

| Term                              | Definition                                                               |
|-----------------------------------|--------------------------------------------------------------------------|
| **In-degree**                     | Number of edges coming into a vertex                                     |
| **Out-degree**                    | Number of edges going out from a vertex                                  |
| **DAG**                           | Directed Acyclic Graph—no cycles when following arrows                   |
| **Strongly Connected Component** | A maximal group where each vertex can reach every other via directed paths |
| **Reachability**                  | Whether you can follow directed edges from one vertex to another         |

---

## Types of Graphs

There are several basic graph types, each defined by edge direction and weight or special structure:

- **Undirected Graph**: edges have no direction, \((u,v)\equiv(v,u)\).  
- **Directed Graph (Digraph)**: edges have a direction \(u\to v\neq v\to u\).  
- **Unweighted Graph**: every edge is treated the same (weight = 1).  
- **Weighted Graph**: edges carry a numerical cost or distance.  
- **Cyclic Graph**: contains at least one cycle.  
- **Acyclic Graph**: contains no cycles.  
- **Tree**: a connected, acyclic undirected graph with \(|E|=|V|-1\).  
- **DAG (Directed Acyclic Graph)**: directed graph with no cycles.  
- **Connected Graph**: an undirected graph with exactly one connected component.  
- **Disconnected Graph**: an undirected graph with two or more separate components.

---


## Tree Properties


A **tree** is a special undirected graph with no cycles and exactly $n-1$ edges on $n$ vertices. We often choose one node as the *root* to discuss:

- **Depth** of a vertex is the number of edges from the root down to that vertex.  
- **Height** of the tree is the maximum depth among all vertices.  
- **Diameter** is the longest shortest-path between any two vertices in the tree.

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
