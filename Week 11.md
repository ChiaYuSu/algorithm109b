# Week 11 write-up
## Administrative matters
- The program assignment 1 (PA1) has been uploaded to [moodle](https://moodle.ntust.edu.tw/), please submit it on time before 12/4
    - All Program assignment submissions will be subject to duplication checking
    - **10% per day** penalty for late submission

## C++ Standard Template Library (STL)
- The reference file has been uploaded to [moodle](https://moodle.ntust.edu.tw/), the program assignment can be used for reference
- The PA1 teaching assistant has helped you write a parser, and PA2 needs to write a parser by yourself

## Unit 5: Greedy Algorithms (貪婪也算法)
- Course contents:
    - Elements of the greedy strategy (貪婪策略的要素)
    - Activity selection (活動選擇問題)
    - Knapsack problem (背包問題)
    - Huffman codes (霍夫曼編碼)
    - Task scheduling (工作排程)
- Greedy algorithm is a **huristic (經驗法則)** algorithm
> **Proof questions (證明題)** will be given during the exam

### Greedy Algorithm: Vertex Cover (頂點覆蓋)
- A vertex cover of an undirected graph *G=(V, E)* is a subset *V' ⊆ V* such that if *(u, v) ∈ E*, then *u ∈ V'* or *v ∈ V'*, or both
    - The set of vertices covers all the edges
- The **size** of a vertex cover is the number of vertices in the cover
<br><img src="Week 11\vertex_cover.PNG" />
- The **vertex-cover problem** is to find a vertex cover of minimum size
in a graph (頂點覆蓋問題是在圖中找到最小尺寸的頂點覆蓋)
- **Greedy heuristic:** cover as many edges as possible (vertex with the maximum degree) at each stage and then delete the covered edges (在每個階段覆蓋盡可能多的邊緣 (最大度的頂點)，然後刪除覆蓋的邊緣)
- **The greedy heuristic cannot always find an optimal solution! (貪婪演算法無法找到最佳解！)**
    - The vertex-cover problem is **NP-complete**

## Activity-Selection Problem (活動選擇問題)
### A Greedy Algorithm
- *A **greedy algorithm** always makes the choice that looks best at the
moment* (貪婪演算法總是會做出目前**看起來**最好的選擇)
- **An Activity-Selection Problem (活動選擇問題):** Given a set *S = {1, 2, …, n}* of *n* proposed activities, with a start time *s<sub>i</sub>* and a finish time *f<sub>i</sub>* for each activity *i*, select a maximum-size set of mutually compatible activities (給定 *n* 個提議的活動的集合 *S = {1,2, …, n}*，並為每個活動 *i* 設置開始時間 *s<sub>i</sub>* 和結束時間 *f<sub>i</sub>*，請選擇相互兼容活動的最大大小集)
    - If selected, activity *i* takes place during the half-open time interval *[s<sub>i</sub>, f<sub>i</sub>)*
    - Activities *i* and *j* are compatible if *[s<sub>i</sub>, f<sub>i</sub>)* and *[s<sub>j</sub>, f<sub>j</sub>)* do not overlap (不重疊) (i.e., *s<sub>i</sub> ≥ f<sub>j</sub>* or *s<sub>j</sub> ≥ f<sub>i</sub>*)
        - e.g.: 
            | *s<sub>i</sub>* | *f<sub>i</sub>* | *s<sub>j</sub>* | *f<sub>j</sub>* | overlap? | *s<sub>i</sub> ≥ f<sub>j</sub>* or *s<sub>j</sub> ≥ f<sub>i</sub>* |
            |----|----|----|----|----------|--------------------|
            | 1  | 4  | 5  | 7  | No       | Yes                |
            | 5  | 7  | 1  | 4  | No       | Yes                |
            | 1  | 5  | 3  | 7  | Yes      | No                 |

### Activity Selection
- Activity selection method:
    1. Sort *f<sub>i</sub>*
    2. Select the first activity
    3. Pick the first activity *i* such that *s<sub>i</sub> ≥ f<sub>j</sub>* where activity *j* is the most recently selected activity
    <br><img src="Week 11\activity_selection.PNG" />

### The Activity-Selection Algorithm
- Pseudo code:
<br><img src="Week 11\greedy_activity_selector.PNG" />
- Trace code:
```
0  Greedy-Activity-Selector(s, f)
1  n = s.length = 4
2  A = {1}
3  j = 1
4  i = 2
5  (s2 = 3) >= (f1 = 4) is False, go to line 8
8  A = {1}, go back to line 4
---
4  i = 3
5  (s3 = 0) >= (f1 = 4) is False, go to line 8
8  A = {1}, go back to line 4
---
4  i = 4
5  (s4 = 5) >= (f1 = 4) is True
6  A = A ∪ {4} = {1, 4}
7  j = i = 4
8  A = {1, 4}
```
> Time complexity **(excluding sorting)**: ***O(n)***
- Graphic explanation:
<br><img src="Week 11\activity_selection_2.PNG" />
- **Theorem:** Algorithm Greedy-Activity-Selector produces solutions of **maximum size** for the activity-selection problem
    1. **Greedy-choice property:** Suppose *A ⊆ S* is an optimal solution. Show that if the first activity in *A* activity *k ≠ 1*, then *B = A - {k} ∪ {1}* is an optimal solution (通過選擇出局部最佳解來獲得全局最佳解)
    2. **Optimal substructure:** Show that if *A* is an optimal solution to *S*, then *A' = A - {1}* is an optimal solution to *S' = {i ∈ S: s<sub>i</sub> ≥ f<sub>1</sub>}* (如果某問題的最佳解包含其子問題的最佳解，則此問題結構為最佳解)

### Optimal Proofs
- **Greedy-choice property:** Suppose *A ⊆ S* is an optimal solution. Show that if the first activity in *A* activity *k ≠ 1*, then *B = A - {k} ∪ {1}* is an optimal solution (假設 *A ⊆ S* 是最佳解。證明如果 *A* 的第一個子問題不包含 1，則 *B = A - {k} ∪ {1}* 是最佳解)
<br><img src="Week 11\greedy_choice_property.PNG" />
- **Optimal substructure:** Show that if *A* is an optimal solution to *S*, then *A' = A - {1}* is an optimal solution to *S' = {i ∈ S: s<sub>i</sub> ≥ f<sub>1</sub>}* (證明如果 *A* 是 *S* 的最佳解，那麼 *A' = A - {1}* 是 S' = {i ∈ S: s<sub>i</sub> ≥ f<sub>1</sub>} 的最佳解)
    - E.g.: *A' = {4, 8, 11}*, *S' = {4, 6, 7, 8, 9, 11}* in the Activity Selection example
    <br><img src="Week 11\optimality_proofs_2.PNG" />
    - **Proof by contradiction (矛盾法):** If *A'* is not an optimal solution to *S'*, we can find a "better" solution *A''*. Then, *A'' ∪ {1}* is a better solution than *A* to *S*, contradicting to the original claim that *A* is an optimal solution to *S*
    <br><img src="Week 11\optimality_proofs_1.PNG" />

### Optimal Proofs (最佳解證明方法)
<img src="Week 11\optimality_proofs_1.PNG" /><br>
1. 已知 *A* 是 *S* 的最佳解
2. 假設 *A'* 不是 *S'* 的最佳解 (反證法)
3. 存在另一個 *A''* 是 *S'* 的最佳解
4. 1 與 3 **矛盾** (實際上 *A'* 與 *A''* 都是最佳解)

### Design A Greedy Algorithm
- We design greedy algorithms according to the following sequence of steps:
    1. **Cast the optimization problem** as one in which we make a choice and are left with one subproblem to solve
    2. **Prove** that there is always an optimal solution to the original problem that makes the **greedy choice**, so that the greedy choice is always safe
    3. **Demonstrate that**, having made the greedy choice, what remains is a subproblem with the property that if we combine an optimal solution to the subproblem with the greedy choice we have made, we arrive at an optimal solution to the original problem

### Elements of the Greedy Strategy
- When to apply greedy algorithms?
    - **Greedy-choice property:** A global optimal solution can be arrived at by making a locally optimal (greedy) choice (通過選擇出局部最佳解來獲得全局最佳解)
        - **Dynamic programming** needs to check the solutions to subproblems (動態規劃演算法會用到此證明法)
    - **Optimal substructure:** An optimal solution to the problem contains within its optimal solutions to subproblems (問題的最佳解包含其子問題的最佳解)
- Greedy ***heuristics*** do not always produce optimal solutions (無法找到最佳解)
- Greedy Algorithms vs. Dynamic Programming (DP)
    | Greedy Algorithm | Dynamic Programming |
    | ---------------- | ------------------- |
    | has **optimal substructure** | has **optimal substructure** |
    | make a greedy choice before solving the subproblem (沒有解掉 subproblem，只是無腦的選擇看起來最佳的解) | make an informed choice after getting optimal solutions to subproblems |
    | **no overlapping** subproblems | **dependent** or **overlapping** subproblems |
    
    <img src="Week 11\DP_vs_Greedy.PNG" /><br>
    > Reference: [Yun-Nung Vivian Chen @ NTU CSIE](https://www.youtube.com/watch?v=y2vumSxk_gE)

## Knapsack Problem (背包問題)
### 0-1 Knapsack Problem
- **Knapsack Problem:** Given *n* items, with *i*th item worth *v<sub>i</sub>* dollars and weighing *w<sub>i</sub>* pounds, a thief wants to take as valuable a load as possible, but can carry at most *W* pounds in his knapsack (將一堆物品儘量塞進背包裡面，使背包裡面的物品總價值最高。背包沒有容量限制，無論物品是什麼形狀大小，都能塞進背包，但是背包有重量限制，如果物品太重，就會撐破背包)
- **The 0-1 knapsack problem:** Each item is either **taken** or **not taken** (0-1 decision)
<br><img src="Week 11\01_knapsack.PNG" />

### Fractional Knapsack Problem **(⭐Compulsory exam)**
- The **fractional** knapsack problem: Allow to take fraction of items (允許使用分數)
- E.g.: 
    - value = 60, 100, 120
    - weight = 10, 20, 30
    - maximum weight (W) = 50
<br><img src="Week 11\01_knapsack.PNG" />
- Greedy solution by taking items in order of greatest value per pound is **optimal for the fractional version, but not for the 0-1 version**
- The 0-1 knapsack problem is **NP-complete**, but can be solved in ***O(nW)*** time by DP **(A polynomial-time DP? → No!!!)**
    - *W = 2<sup>k</sup>* → *O(n · 2<sup>k</sup>)*
    - *2<sup>k</sup>* → 0 or 1
    - *O(nW)*: **pseudo polynomial**

## Huffman Codes (霍夫曼編碼)
### Coding (編碼)
- Is used for data compression (數據壓縮), instruction-set encoding (指令集編碼), etc
- **Binary character code:** character is represented by a unique binary string
    - **Fixed-length code (block code):** ***a*: 000**, *b*: 001, …, *f*: 101
        - ***a**ce* → 000 010 100
    - **Variable-length code:**
        - frequent characters → short codeword (出現頻率較**多**次的，使用**短**編碼)
            - (45 + 13 + 12 + 16 + 9 + 5) · 3 = 300
        - infrequent characters → long codeword (出現頻率較**少**次的，使用**長**編碼)
            - 45 · 1 + (13 + 12 + 16) · 3 + (9 + 5) · 4 = 244
        <br><img src="Week 11\huffman_coding.PNG" />

### Binary Tree vs. Prefix (字首) Code
- **Prefix codes:** No codeword is a prefix of some other codeword
    - A binary tree whose leaves are the given characters can be served as a representation of prefix codes
    <br><img src="Week 11\prefix_code.PNG" />
        - Not prefix code: *C:* 10 is *B:* 101 prefix
            - Ambiguity (模稜兩可): decode(1011100) can be **'BF' (101-1100)** or **'CDAA' (10-111-0-0)**
            > prefix codes are uniquely decodable

### Optimal Prefix Code Design
- **Coding Cost** of *T*: <img src="Week 11\tree_cost.PNG" height="30px" />
    - *c*: charcter in the alphabet *C*
    - *c.freq*: frequency of *c*
    - *d<sub>T</sub>(c)*: depth of *c*'s leaf (length of the codeword of *c*)
- **Code design:** Given *c<sub>1</sub>.freq*, *c<sub>2</sub>.freq*, ..., *c<sub>n</sub>.freq*, construct a binary tree with *n* leaves such that *B(T)* is minimized
    - **Idea: more frequently used characters use shorter depth**

### Huffman's Procedure
- **Pair** two nodes with the least costs at each step
<br><img src="Week 11\huffman_procedure.PNG" />

### Huffman's Algorithm
- Pseudo code:
<br><img src="Week 11\huffman_pseudo_code.PNG" />
- Trace code:
```
0  Huffman(C = 6)
1  n = |C| = 6
2  Q = C = {5, 9, 12, 13, 16, 45}
3  i = 1
4  new node z
5  z.left = x = 5
6  z.right = y = 9
7  z.freq = 5 + 9 = 14
8  Q = {12, 13, 14, 16, 45}, go back to line 3
---
3  i = 2
4  new node z
5  z.left = x = 12
6  z.right = y = 13
7  z.freq = 12 + 13 = 25
8  Q = {14, 16, 25, 45}, go back to line 3
---
3  i = 3
4  new node z
5  z.left = x = 14
6  z.right = y = 16
7  z.freq = 14 + 16 = 30
8  Q = {25, 30, 45}, go back to line 3
---
3  i = 4
4  new node z
5  z.left = x = 25
6  z.right = y = 30
7  z.freq = 25 + 30 = 55
8  Q = {45, 55}, go back to line 3
---
3  i = 5
4  new node z
5  z.left = x = 45
6  z.right = y = 55
7  z.freq = 45 + 55 = 100
8  Q = {100}
9  return the root of the tree = 100
```
- Pseudo code annotation:
    - line 2: Q means **a min-priority queue**
- Graphic explanation:
<br><img src="Week 11\huffman_procedure.PNG" />
- Time complexity: *O(n lg n)*
    - Extract-Min(*Q*) and Insert(*Q*, *z*) need *O(lg n)* by a **heap** operation
    - Requires initially ***O(n lg n)*** time to build a binary heap

### Huffman's Algorithm: Greedy Choice
- **Greedy choice:** two characters *x* and *y* with the lowest frequencies must have *the same length* and differ only in the last bit (頻率最低的兩個字符 *x* 和 *y* 必須具有相同的位元長度，並且僅在最後一位不同)
- Proof via contradiction (矛盾法)
    - Assume that there is no OPT including this greedy choice → tree (*T*)
        - *x* and *y* are 2 symbols with lowest frequencies
        - *b* and *c* are siblings with largest depths
        - *x.freq* ≤ *b.freq*
    - Swap *b* and *x* → new tree (*T'*)
        - ***B(T) ≥ B(T')***
        - *y.freq* ≤ *c.freq*
    - Swap *c* and *y* → new tree (*T''*)
        - ***B(T') ≥ B(T'')***
        <br><img src="Week 11\greedy_choice.PNG" />

### Huffman's Algorithm: Optimal Substructure
- Optimal substructure: Let *T* be a binary tree for an optimal prefix code over *C*. Let *z* be the parent of two leaf characters *x* and *y*. If *z.freq* = *x.freq* + *y.freq*, tree *T' = T - {x, y}* represents an optimal prefix code for *C' = C - {x, y} ∪ {z}*
- Proof via contradiction (矛盾法)
    1. Given optimal tree *T*
        - *B(T) = B(T') + x.freq + y.freq*
        - *depth(x) = depth(z) + 1*
        - *depth(y) = depth(z) + 1*
    2. Assume that another tree *T'* is not optimal
    3. If *T'* is not optimal, find *T''* subject to (滿足) *B(T'') ≤ B(T')*
        - *z* in *C'*
            - *z* is a leaf of *T''*
        - Add *x, y* as *z*'s children of *T'''* 
            - ***B(T''') = B(T'') + x.freq + y.freq** < **B(T') + x.freq + y.freq = B(T)*** **(contradiction)**