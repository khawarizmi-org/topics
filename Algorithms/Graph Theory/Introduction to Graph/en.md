# Introduction to Graph

A **Graph** is a data structure consisting of a set of **nodes** (also called **vertices**) and a set of **edges** connecting them. Graphs are fundamental in competitive programming for problems involving connectivity, shortest paths, components, and more.

---

## Key Concept

* A graph \( G = (V, E) \) contains:
  * \( V \): a set of vertices (nodes)
  * \( E \): a set of edges (connections between pairs of nodes)
* Edges can be **directed** or **undirected**
* Edges can be **weighted** or **unweighted**
* Graphs can be represented using an **adjacency list** or an **adjacency matrix**

---

## Terminology

| Term             | Meaning                                                        |
|------------------|----------------------------------------------------------------|
| Vertex (Node)    | A point or object in the graph                                 |
| Edge             | A connection between two vertices                              |
| Adjacent         | Two vertices are adjacent if connected by an edge              |
| Degree           | Number of edges connected to a vertex                          |
| Path             | A sequence of vertices connected by edges                      |
| Cycle            | A path that starts and ends at the same vertex                 |
| Connected Graph  | Every vertex is reachable from every other vertex              |
| Tree             | A connected, undirected, acyclic graph with \(n - 1\) edges    |
| DAG              | Directed Acyclic Graph (no cycles)                             |

---

## Types of Graphs

1. **Undirected Graph**:  
   Edges have no direction — \( (u, v) = (v, u) \)

2. **Directed Graph**:  
   Edges have direction — \( (u, v) \neq (v, u) \)

3. **Unweighted Graph**:  
   All edges are treated equally

4. **Weighted Graph**:  
   Each edge has a weight (cost, distance, etc.)

5. **Cyclic Graph**:  
   Contains at least one cycle

6. **Acyclic Graph**:  
   No cycles

7. **Tree**:  
   Undirected, connected, acyclic graph with \( n \) nodes and \( n - 1 \) edges

8. **DAG**:  
   Directed Acyclic Graph, used in topological sorting and scheduling

---

## Adjacency List

- Stores a list of neighbors for each node
- Space complexity: \(O(n + m)\)
- Edge check complexity: \(O(\text{degree})\)


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

- 2D matrix of size \( n \times n \)
- \( \text{adj}[u][v] = 1 \) means there is an edge from node \( u \) to node \( v \)
- **Space complexity**: \( O(n^2) \)
- **Edge check complexity**: \( O(1) \)

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