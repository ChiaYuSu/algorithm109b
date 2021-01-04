# Week 13 write-up
## Unit 7: Graphs
- Course contents:
    - Elementary graph algorithms (基本圖演算法)
    - Minimum spanning trees (最小生成樹)
    - Shortest paths (最短路徑)
    - Maximum flow (最大流量)

### Graphs
- A **graph** *G = (V, E)* consists of a set *V* of **vertics (nodes)** and a set *E* of **directed** or **undirected edges**
    - For analysis, we use *V* for |*V*| and E for |*E*|
- Any **binary** relation is a graph
    - Network of roads and cities, circuit (電路) representation, etc
    <br><img src="Week 13\Graphs.PNG" />

### Representations of Graphs: Adjacency List
- **Adjacency (相鄰) list:** An array *Adj* of |*V*| lists, one for each vertex in *V*. For each *u ∈ V*, *Adj[u]* points to all the vertices adjacent to *u*
- Advantage: ***O(V+E)*** storage, good for **sparse (鬆散)** graphs
    - Sparse graphs: *|E| = O(V)*
    - Dense graphs: *|E| = O(V<sup>2</sup>)*
- Drawback (缺點): Need to traverse list to find an edge
<br><img src="Week 13\adjacency_list.PNG" />

### Representations of Graphs: Adjacency Matrix
- **Adjacency matrix:** A *|V| * |V|* matrix *A = (a<sub>ij</sub>)* such that
<br><img src="Week 13\adjacency_matrix.PNG" height="45px"/>
- Advantage: *O(1)* time to find an edge
- Drawback: *O(V<sup>2</sup>)* storage, more suitable for **dense (稠密)** graphs
    - Sparse graphs: *|E| = O(V)*
    - Dense graphs: *|E| = O(V<sup>2</sup>)*
- How to save space if the graph is undirected?
    - The matrix will be **symmetrical (對稱的)**
    <br><img src="Week 13\adjacency_matrix_2.PNG" />

### Tradeoffs (權衡) between Adjacency List and Matrix
- Faster to find an edge?
    - Matrix
- Faster to find vertex degree (度)?
    - List
> In graph theory, the **degree** of a vertex in the graph is the number of edges connected to the vertex (在圖論中，一個頂點在圖中的**度**為與這個頂點相連接的邊的數目)
- Faster to traverse the graph?
    - List: *O(V+E)* **(Faster)**
    - Matrix: *O(V<sup>2</sup>)*
- Storage for sparse graph?
    - List: *O(V+E)* **(More suitable)**
    - Matrix: *O(V<sup>2</sup>)* 
- Storage for dense graph?
    - List: *O(V+E)*
    - Matrix: *O(V<sup>2</sup>)* **(More suitable)**
- Edge insertion or deletion?
    - Matrix: *O(1)*
- Weight graph implementation?
    - Depending on the actual situation (看實作情形而定)
- **Better for most applications?**
    - **List**, because most of the application use sparse graph

## Breadth-First Search and Depth-First Search (廣度優先搜尋和深度優先搜尋)
### Breadth-First Search (BFS)
- Pseudo code:
<br><img src="Week 13\BFS.PNG" />
- Trace code:
```
0   BFS(Graph, source)
1   for each vertex u ∈ G.V - {s}
2       u.color = WHITE
3       u.d = ∞
4       u.𝝿 = NIL
5   s.color = GRAY
6   s.d = 0
7   s.𝝿 = NIL
8   Q = ∅
9   Enqueue(Q, s) → Q = {s}
10  (Q != ∅) is True
11  Dequeue[Q] = s → Q = {}
12  (r ∈ G.Adj[s]) is True
13  (r.color == WHITE) is True
14  r.color = GRAY
15  r.d = s.d + 1 = 1
16  r.𝝿 = s
17  Enqueue(Q, r) → Q = {r}, go back to line 12
-----
12  (w ∈ G.Adj[s]) is True
13  (w.color == WHITE) is True
14  w.color = GRAY
15  w.d = s.d + 1 = 1
16  w.𝝿 = s
17  Enqueue(Q, w) → Q = {r, w}, s has no more adjacency vertex, go to line 18
18  s.color = BLACK, go back to line 10
-----
10  (Q != ∅) is True
11  Dequeue[Q] = r → Q = {w}
12  (s ∈ G.Adj[r]) is True
13  (s.color == WHITE) is False, go back to line 12
-----
12  (v ∈ G.Adj[r]) is True
13  (v.color == WHITE) is True
14  v.color = GRAY
15  v.d = r.d + 1 = 2
16  v.𝝿 = r
17  Enqueue(Q, v) → Q = {w, v}, r has no more adjacency vertex, go to line 18
18  r.color = BLACK, go back to line 10
-----
10  (Q != ∅) is True
11  Dequeue[Q] = w → Q = {v}
12  (s ∈ G.Adj[w]) is True
13  (s.color == WHITE) is False, go back to line 12
-----
12  (t ∈ G.Adj[w]) is True
13  (t.color == WHITE) is True
14  t.color = GRAY
15  t.d = w.d + 1 = 2
16  t.𝝿 = w
17  Enqueue(Q, t) → Q = {v, t}
-----
12  (x ∈ G.Adj[w]) is True
13  (x.color == WHITE) is True
14  x.color = GRAY
15  x.d = w.d + 1 = 2
16  x.𝝿 = w
17  Enqueue(Q, x) → Q = {v, t, x}, w has no more adjacency vertex, go to line 18
18  w.color = BLACK, go back to line 10
-----
10  (Q != ∅) is True
11  Dequeue[Q] = v → Q = {t, x}
12  (r ∈ G.Adj[v]) is True
13  (r.color == WHITE) is False, v has no more adjacency vertex, go to line 18
18  v.color = BLACK, go back to line 10
-----
10  (Q != ∅) is True
11  Dequeue[Q] = t → Q = {x}
12  (w ∈ G.Adj[t]) is True
13  (w.color == WHITE) is False, go back to line 12
-----
12  (x ∈ G.Adj[t]) is True
13  (x.color == WHITE) is False, go back to line 12
-----
12  (u ∈ G.Adj[t]) is True
13  (u.color == WHITE) is True
14  u.color = GRAY
15  u.d = t.d + 1 = 3
16  u.𝝿 = t
17  Enqueue(Q, u) → Q = {x, u}, t has no more adjacency vertex, go to line 18
18  t.color = BLACK, go back to line 10
-----
10  (Q != ∅) is True
11  Dequeue[Q] = x → Q = {u}
12  (w ∈ G.Adj[x]) is True
13  (w.color == WHITE) is False, go back to line 12
-----
12  (t ∈ G.Adj[x]) is True
13  (t.color == WHITE) is False, go back to line 12
-----
12  (u ∈ G.Adj[x]) is True
13  (u.color == WHITE) is False, go back to line 12
-----
12  (y ∈ G.Adj[x]) is True
13  (y.color == WHITE) is True
14  y.color = GRAY
15  y.d = x.d + 1 = 3
16  y.𝝿 = x
17  Enqueue(Q, y) → Q = {u, y}, x has no more adjacency vertex, go to line 18
18  x.color = BLACK, go back to line 10
-----
10  (Q != ∅) is True
11  Dequeue[Q] = u → Q = {y}
12  (t ∈ G.Adj[u]) is True
13  (t.color == WHITE) is False, go back to line 12
-----
12  (x ∈ G.Adj[u]) is True
13  (x.color == WHITE) is False, go back to line 12
-----
12  (y ∈ G.Adj[u]) is True
13  (y.color == WHITE) is False, u has no more adjacency vertex, go to line 18
18  u.color = BLACK, go back to line 10
-----
10  (Q != ∅) is True
11  Dequeue[Q] = y → Q = {}
12  (x ∈ G.Adj[y]) is True
13  (x.color == WHITE) is False, go back to line 12
-----
12  (u ∈ G.Adj[y]) is True
13  (u.color == WHITE) is False, y has no more adjacency vertex, go to line 18
18  y.color = BLACK, go back to line 10
-----
10  (Q != ∅) is False, Finally complete
```
- Pseudo code annotation:
    - Enqueue: Insert a data from the end (從**尾端插入**一個資料)
    - Dequeue: Take out a data from the front (從**前端取出**一個資料)
    - Line 0-7: Initial the graph
    - color means:
        - white (**undiscovered**)
        - gray (**discovered**)
        - black (**explored:** out edges are all discovered)
    - `?.d`: distance from source *s*
    - `?.𝝿`: predecessor of ?
    - **Use queue for gray vertices**
    - **Time complexity: *O(V+E)* (adjacency list)**
        - **Each vertex is enqueued and dequeued once: *O(V)* time**
        - **Each edge is considered once: *O(E)* time**
    - Breadth-first tree:
        - *G<sub>𝝿</sub> = (V<sub>𝝿</sub>, E<sub>𝝿</sub>)*
        - *V<sub>𝝿</sub> = {v ∈ V | 𝝿[v] ≠ NIL} ∪ {s}*
        - *E<sub>𝝿</sub> = {(𝝿[v], v) ∈ E | v ∈ V<sub>𝝿</sub> - {s}}*
- Graphic explanation:
<br><img src="Week 13\BFS_graph.PNG" height="1300px"/>

### Depth-First Search (DFS)
- Pseudo code:
<br><img src="Week 13\DFS.PNG" />
- Trace code:
```
0   DFS(Graph)
1   for each vertex u ∈ G.V
2       u.color = WHITE
3       u.𝝿 = NIL
4   time = 0
5   (u ∈ G.V) is True
6   (u.color == WHITE) is True
7   Call DFS-Visit(Graph, u)
-----
0   DFS-Visit(Graph, u)
1   time = 1
2   u.d = 1
3   u.color = GRAY
4   (v ∈ G.Adj[u]) is True
5   (v.color == WHITE) is True
6   v.𝝿 = u
7   Call DFS-Visit(Graph, v)
-----
0   DFS-Visit(Graph, v)
1   time = 2
2   v.d = 2
3   v.color = GRAY
4   (y ∈ G.Adj[v]) is True
5   (y.color == WHITE) is True
6   y.𝝿 = v
7   Call DFS-Visit(Graph, y)
-----
0   DFS-Visit(Graph, y)
1   time = 3
2   y.d = 3
3   y.color = GRAY
4   (x ∈ G.Adj[y]) is True
5   (x.color == WHITE) is True
6   x.𝝿 = y
7   Call DFS-Visit(Graph, x)
-----
0   DFS-Visit(Graph, x)
1   time = 4
2   x.d = 4
3   x.color = GRAY
4   (v ∈ G.Adj[x]) is True
5   (v.color == WHITE) is False, go to line 8
8   x.color = BLACK
9   time = 5
10  x.f = 5, return back to DFS line 5
-----
5   (y ∈ G.V) is True
6   (y.color == WHITE) is False
    y.color = BLACK
    y.f = 6
    go back to line 5
-----
5   (v ∈ G.V) is True
6   (v.color == WHITE) is False
    v.color = BLACK
    v.f = 7
    go back to line 5
-----
5   (u ∈ G.V) is True
6   (u.color == WHITE) is False
    u.color = BLACK
    u.f = 8
    go back to line 5
-----
5   (w ∈ G.V) is True
6   (w.color == WHITE) is True
7   Call DFS-Visit(Graph, w)
-----
0   DFS-Visit(Graph, w)
1   time = 9
2   w.d = 9
3   w.color = GRAY
4   (y ∈ G.Adj[w]) is True
5   (y.color == WHITE) is False, go back to line 4
-----
4   (z ∈ G.Adj[w]) is True
5   (z.color == WHITE) is True
6   z.𝝿 = w
7   Call DFS-Visit(Graph, z)
-----
0   DFS-Visit(Graph, z)
1   time = 10
2   z.d = 10
3   z.color = GRAY
4   (z ∈ G.Adj[z]) is True
5   (z.color == WHITE) is False
8   z.color = BLACK
9   time = 11
10  z.f = 11, return back to DFS line 5
-----
5   (w ∈ G.V) is True
6   (w.color == WHITE) is False
    w.color = BLACK
    w.f = 12
    Finally complete
```
- Pseudo code annotation:
    - `DFS(G)` Line 1-4: Initial the graph
    - color means:
        - white (**undiscovered**)
        - gray (**discovered**)
        - black (**explored:** out edges are all discovered) 
    - `?.d`: discovery time **(gray)**
    - `?.f`: finishing time **(black)**
    - `?.𝝿`: predecessor
    - **Time complexity: *O(V+E)* (adjacency list)**
    - Depth-first forest:
        - *G<sub>𝝿</sub> = (V, E<sub>𝝿</sub>)*
        - *E<sub>𝝿</sub> = {(𝝿[v], v) ∈ E | v ∈ V, 𝝿[v] ≠ NIL}*
- Graphic explanation:
<br><img src="Week 13\DFS_graph.PNG" />
<br><img src="Week 13\DFS_graph_2.PNG" />

### BFS Application: Lee's Maze Router (繞線)
- Find a path from S to T by **wave propagation**
- Discuss mainly on single-layer (單層) routing
- Strength: Guarantee to find a minimum-length connection between 2 terminals if it exists (保證在兩個端子之間找到**最小長度**的連接，如果存在的話)
- Weakness: Time & space complexity for an *M * N* grid: *O(MN)* **(huge!!!)**
<br><img src="Week 13\Lee_Maze.PNG" />

### BFS + DFS: Soukup's Maze Router
- Depth-first (line) search is first directed toward target *T* until an obstacle or *T* is reached (深度優先搜尋首先指向目標 *T*，直到碰到障礙物或 *T*)
- Breadth-first (Lee-type) search is used to **bubble around** an obstacle if an obstacle is reached (如果碰到障礙物，則使用廣度優先搜尋在障礙物周圍 Bubble)
- Time and space complexities: *O(MN)*, but 10-50 times faster than Lee's algorithm
- Find a path between *S* and *T*, but **may not be the shortest** (不保證一定是最短路徑)

## Topological Sort and Strongly Connected Component
### Topological Sort
- A topological sort of a **directed acyclic graph (DAG) (有向無圈)** *G = (V, E)* is a linear ordering of *V* subject to (滿足) *(u, v) ∈ E → u* appears before *v*
    - Directed cyclic graph:
    <br><img src="Week 13\directed_acyclic.PNG" />
    - A directed graph *G* is acyclic ⟺ *G* has no back edge
    - No linear ordering is possible if a graph is not acyclic (如果圖是循環的，則不可能進行線性排序)
    - **Time complexity: *O(V+E)* (adjacency list)**

### Topological Sort (cont'd)
- Pseudo code:
<br><img src="Week 13\topological.PNG" />
- Graphic explanation:
<br><img src="Week 13\topological_graph.PNG" />
    - Vertices are arranged from **left to right** in order of **decreasing** finishing times

### Strongly Connected Component (SCC)
- A **strongly connected component (SCC)** of a directed graph *G = (V, E)* is a **maximal** set of vertices *U ⊆ V* subject to *u → v, v → u* (*u* and *v* are reachable from each other)
    - *G<sup>T</sup> = (V, E<sup>T</sup>)*: transpose of *G*, where *E<sup>T</sup> = {(u, v):(v, u) ∈ E}*
    - *u* and *v* are reachable from each other in *G* ⟺ *u* and *v* are reachable from each other in *G<sup>T</sup>*
    - Time complexity: *O(V+E)* (adjacency list)
- **There must be no cycle between different SCCs. If there is a cycle, the two components must be merged (不同 SCC 之間一定不存在 cycle，如果存在 cycle 須將兩個 components 合併)**

### SCC Example
- Pseudo code:
<br><img src="Week 13\SCC.PNG" />
- Trace code:
```
1  call DFS(G) to compute finishing time u.f, and sort u.f from big to small
2  call DFS(Transpose G), but consider the verticies in order of decreasing u.f
3  output the verticies of each tree in the depth-first forest of step 2 as a separate strongly connected component
```
- Graphic explanation:
<br><img src="Week 13\SCC_graph.PNG" />

## Minimum Spanning Tree (MST) (最小生成樹)
### Minimum Spanning Tree (MST)
- Given an undirected graph *G = (V, E)* with weights on the edges, a **minimum spanning tree (MST)** of *G* is a subset *T ⊆ E* such that
    - *T* is connected and has no cycles (沒有循環)
    - *T* covers (spans) all vertices in *V* (*T* 覆蓋 *V* 中的所有頂點)
    - sum of the weights of all edges in *T* is minimum (*T* 中所有邊的權重之和最小)
- |*T*| = |*V*| - 1
- Applications: **circuit interconnection (電路互連)** (minimizing tree radius), **communication network** (minimizing tree diameter), etc
<br><img src="Week 13\MST.PNG" />

### Growing a Minimum Spanning Tree (MST)
- Grows an MST by adding one safe edge at a time (通過一次添加一個安全邊來增加 MST)
    - The addition of a safe edge makes *A* a subset of some minimum spanning tree (建立一個用來收集「所有 MST 中的 edge」之集合，稱為 Set *A*)
- A cut (*S, V-S*) of a graph *G = (V, E)* is a partition of *V*
- An edge *(u, v) ∈ E* crosses the cut *(S, V-S)* if one of its endpoints is in *S* and the other is in *V-S*
- A cut respects the set *A* of edges if no edges in *A* crosses the cut
- An edge is a light edge crossing a cut if **its weight is the minimum of any edge crossing the cut**
- **A light edge crossing the cut (*S, V-S*) is safe for *A***


### Example of MST
- Find the minimum spanning tree:
<br><img src="Week 13\find_MST.PNG" />
- Cut property:
<br><img src="Week 13\find_MST_cut.PNG" />
- Crossing edge:
<br><img src="Week 13\find_MST_crossing_edge.PNG" />
- Light edge:
<br><img src="Week 13\find_MST_light_edge.PNG" />

### Kruskal's MST Algorithm
- Kruskal's MST Algorithm is the most famous MST Algorithm
- Pseudo code:
<br><img src="Week 13\kruskal.PNG" />
- Pseudo code annotation:
    - Add a safe edge at a time to the growing forest by finding an edge of least weight (from those connecting two trees in the forest)
    - Line 6: `Find-Set(u) ≠ Find-Set(v)` (節點 *u* 和節點 *v* 不在同一個子樹)
- Graphic explanation:
<br><img src="Week 13\kruskal_graph.PNG" />

### Prim's (Prim-Dijkstra's?) MST Algorithm
- Pseudo code:
<br><img src="Week 13\MST_Prim.PNG" />
- Trace code:
```
0   MST-Prim(G, w, r)
1   for each vertex u ∈ G.V
2   u.key = ∞
3   u.𝝿 = NIL
4   r.key = 0
5   Q = G.V
6   while Q ≠ ∅
7   
```

 