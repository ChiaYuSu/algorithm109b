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
- Graphic explanation:
<br><img src="Week 13\BFS_graph.PNG" />
