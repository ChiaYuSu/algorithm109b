# Week 9 write-up
## Administrative matters
- Next week is the midterm exam, please go back and practice the exercises you did on the previous homework, otherwise you won't be able to finish it
- In order to provide a more comfortable examination environment for everyone, the midterm exam will be held at IB-501

## Longest Common Subsequence (LCS Algorithm)
- A very famous algorithm
- Teachers find the **LCS algorithm to be the best of all DP (Dynamic Programming) algorithms**

### Longest Common Subsequence
- **Problem:** Given *X* = *<x<sub>1</sub>, x<sub>2</sub>, ..., x<sub>m</sub>>* and *Y* = *<y<sub>1</sub>, y<sub>2</sub>, ..., y<sub>m</sub>>*, find the **longest common subsequence (LCS)** of *X* and *Y*
    - E.g.: *X* = *<a, b, c, b, d, a, b>* and *Y* = *<b, d, c, a, b, a>*, **LCS = *<b, c, b, a>* (also, LCS = *<b, d, a, b>*)**
    - E.g.: DNA sequencing:
        - S1 = ACCG-GTCG-AGAT-GCAG
        - S2 = GTCG-TTCG-GAAT-GCAT
        - LCS = CGTC-GGAT-GCA
- Brute-force method (暴力法):
    - Enumerate all subsequences of *X* and check if they appear in *Y* (枚舉 *X* 的所有子序列，並檢查它們是否出現在 *Y* 中)
    - Each subsequence of *X* corresponds to a subset of the indices {1, 2, …, *m*} of the elements of *X* (*X* 的每個子序列對應於 *X* 元素的索引 {1, 2, …, *m*} 的子集)
    - There are 2<sup>*m*</sup> subsequences of *X*. Why?
        - 對應過程每個元素都有兩種可能性 (**有**或**沒有**)，然後一共有 *m* 個元素，所以答案為 2<sup>*m*</sup>

### Optimal Substructure for LCS (LCS 最佳子結構)
- Let *X<sub>m</sub>* = *<x<sub>1</sub>, x<sub>2</sub>, ..., x<sub>m</sub>>* and *Y<sub>n</sub>* = *<y<sub>1</sub>, y<sub>2</sub>, ..., y<sub>n</sub>>* be sequences, and *Z<sub>k</sub>* = *<z<sub>1</sub>, z<sub>2</sub>, ..., z<sub>k</sub>>* be LCS of *X<sub>m</sub>* and *Y<sub>n</sub>*
    - Case 1: *x<sub>m</sub>* = *y<sub>n</sub>*
    <br><img src="Week 9\LCS_Case_1.PNG" width="350px" />
    - Case 2: *x<sub>m</sub>* ≠ *y<sub>n</sub>*
        - *z<sub>k</sub>* may not be *x<sub>m</sub>*
        <br><img src="Week 9\LCS_Case_2_1.PNG" width="200px" />
        - *z<sub>k</sub>* may not be *y<sub>n</sub>*
        <br><img src="Week 9\LCS_Case_2_2.PNG" width="200px" />
- Let *X* = *<x<sub>1</sub>, x<sub>2</sub>, ..., x<sub>m</sub>>* and *Y* = *<y<sub>1</sub>, y<sub>2</sub>, ..., y<sub>n</sub>>* be sequences, and *Z* = *<z<sub>1</sub>, z<sub>2</sub>, ..., z<sub>k</sub>>* be LCS of *X* and *Y*
    1. If *x<sub>m</sub>* = *y<sub>n</sub>*, then *z<sub>k</sub> = x<sub>m</sub>* = *y<sub>n</sub>* and *Z<sub>k-1</sub>* is an LCS of *X<sub>m-1</sub>* and *Y<sub>n-1</sub>*
    2. If *x<sub>m</sub>* ≠ *y<sub>n</sub>*, then *z<sub>k</sub> ≠ x<sub>m</sub>* implies *Z* is an LCS of *X<sub>m-1</sub>* and *Y*
    3. If *x<sub>m</sub>* ≠ *y<sub>n</sub>*, then *z<sub>k</sub> ≠ y<sub>n</sub>* implies *Z* is an LCS of *X* and *Y<sub>n-1</sub>*
    <br><img src="Week 9\LCS.PNG" width="550px" />
    - *c[i, j]*: length of the LCS of *X<sub>i</sub>* and *Y<sub>j</sub>*
    - *c[i-1, j-1] + 1*: length of the LCS of *X* and *Y*
    - Basis: *c[0, j] = 0* and *c[i, 0] = 0*

### Bottom-Up DP for LCS
- Find the right order to solve the subproblems (找出正確的順序來解決子問題)
- To compute *c[i, j]*, we need *c[i-1, j-1]*, *c[i-1, j]*, and *c[i, j-1]*
- *b[i, j]*: points to the table entry with reference to the optimal subproblem solution chosen when computing *c[i, j]*
- *c[i, j]*: length of the LCS of *X<sub>i</sub>* and *Y<sub>j</sub>*
- Pseudo code:
<br><img src="Week 9\LCS_Pseudo_code.PNG" width="350px" />
- Trace code:
```
0   LCS-Length(X, Y)
1   m = X.length = 7
2   n = Y.length = 6
3   b[1 ... 7, 1 ... 6], c[0 ... 7, 0 ... 6]
4   i = 1
5   c[1, 0] = 0, go back to line 4
-----
4   i = 2
5   c[2, 0] = 0, go back to line 4
-----
and so on...
-----
4   i = 7
5   c[7, 0] = 0
6   j = 0
7   c[0, 0] = 0, go back to line 6
-----
6   j = 1
7   c[0, 1] = 0, go back to line 6
-----
and so on...
-----
6   j = 6
7   c[0, 6] = 0
8   i = 1
9   j = 1
10  (A == B) is False, go to line 13
13  (c[0, 1]=0 >= c[1, 0]=0) is True
14  c[1, 1] = c[0, 1]
15  b[1, 1] = ↑, go back to line 8
-----
and so on...
-----
18  return table c and table b
```
- Pseudo code annotation:
    - line 4 - 5: 將最左側那一欄都填上 0
    - line 6 - 7: 將最上側那一列都填上 0
    - line 8 - line 17: 填完剩下的其他格子
- Graphic explanation:
<br><img src="Week 9\LCS_Graphic.PNG" width="350px" />

### Example of LCS
- LCS time and space complexity: *O(mn)*
- *X* = *<A, B, C, B, D, A, B>* and *Y* = *<B, D, C, A, B, A>* → LCS = *<B, C, B, A>*
<br><img src="Week 9\LCS_Graphic.PNG" width="350px" />

### Constructing an LCS (構造 LCS)
- Trace back from *b[m, n]* to *b[1, 1]*, following the arrows: *O(m+n)* time
- Pseudo code:
<br><img src="Week 9\LCS_Trace_back_Pseudo_code.PNG" width="300px" />
- Trace code:
```
0  Print-LCS(table b, X, i = 7, j = 6)
1  (i == 0 or j == 0) is False (i = 7, j = 6), go to line 3
3  (b[7, 6] == ↖) is False (↑), go to line 6
6  (b[7, 6] == ↑) is True
7  Call Print-LCS(table b, X, i = 6, j = 6), go back to line 1
-----
1  (i == 0 or j == 0) is False (i = 6, j = 6), go to line 3
3  (b[6, 6] == ↖) is True
4  Call Print-LCS(table b, X, i = 5, j = 5), go back to line 1
5  print X6 = A
-----
and so on...
```
- Graphic explanation:
<br><img src="Week 9\LCS_Trace_back_Graphic.PNG" width="350px" />

### Top-Down DP for LCS
- *c[i, j]*: length of the LCS of *X<sub>i</sub>* and *Y<sub>j</sub>*, where *X<sub>i</sub>* = *<x<sub>1</sub>, x<sub>2</sub>, ..., x<sub>i</sub>>* and *Y<sub>j</sub>* = *<y<sub>1</sub>, y<sub>2</sub>, ..., y<sub>j</sub>>*
- *c[m, n]*: LCS of *X* and *Y*
- Basis: *c[0, j]* = 0 and *c[i, 0]* = 0
<br><img src="Week 9\LCS.PNG" width="550px" />
- The top-dowm dynamic programming: initialize *c[i, 0]* = *c[0, j]* = 0, *c[i, j]* = NIL (將最左側那一欄都填上 0、將最上側那一列都填上 0，剩下的其他格子設定為 NIL)
- Pseudo code:
<br><img src="Week 9\LCS_Top_down_Pseudo_code.PNG" width="550px" />
- Trace code:
```
0  TD-LCS(i = 1, j = 1)
1  (c[1, 1] == NIL) is False
2  (A == B) is False, go to line 4
4  c[1, 1] = c[1, 0] = 0, go back to line 1
-----
1  (c[1, 2] == NIL) is False
2  (A == D) is False, go to line 4
4  c[1, 2] = c[1, 1] = 0, go back to line 1
-----
and so on...
```
- Graphic explanation:
<br><img src="Week 9\LCS_Graphic.PNG" width="350px" />

## Optimal Binary Search Trees (最佳二元搜尋樹)
### Optimal Binary Search Tree
- Application: **English dictionary**
- Given a sequence *K* = *<k<sub>1</sub>, k<sub>2</sub>, ..., k<sub>n</sub>>* of *n* distinct (不同) keys in sorted order (*k<sub>1</sub> < k<sub>2</sub> < ... < k<sub>n</sub>*) and a set of probabilities (機率) *P* = *<p<sub>1</sub>, p<sub>2</sub>, ..., p<sub>n</sub>>* for searching the keys in *K* and *Q* = *<q<sub>0</sub>, q<sub>1</sub>, q<sub>2</sub>, ..., q<sub>n</sub>>* for unsuccessful searches (不成功的搜尋) (corresponding to *D* = *<d<sub>0</sub>, d<sub>1</sub>, d<sub>2</sub>, ..., d<sub>n</sub>>*) of ***n+1*** distinct dummy keys with *d<sub>i</sub>* representing all values between *k<sub>**i**</sub>* and *k<sub>i+1</sub>*), construct a binary search tree whose expected search cost is smallest
<br><img src="Week 9\Optimal_BST.PNG" width="550px" />

### An Example
- This table question will give:
    | *i*             | 0    | 1    | 2    | 3    | 4    | 5    |
    |-----------------|------|------|------|------|------|------|
    | *p<sub>i</sub>* |      | 0.15 | 0.10 | 0.05 | 0.10 | 0.20 |
    | *q<sub>i</sub>* | 0.05 | 0.10 | 0.05 | 0.05 | 0.05 | 0.10 |

    - All *p<sub>i</sub>* values + *q<sub>i</sub>* values = 1
- The expected search cost of the best binary search tree (最佳二元搜尋樹的期望搜索成本):
    - **cost * probability = expected value**
    - **cost = depth + 1**

    | node            | depth | probability | contribution *((depth+1) * probability)* |
    |-----------------|-------|-------------|------------------------------------------|
    | *k<sub>1</sub>* | 1     | 0.15        | 0.30 → *(1+1) * 0.15*                    |
    | *k<sub>2</sub>* | 0     | 0.10        | 0.10 → *(0+1) * 0.10*                    |
    | *k<sub>3</sub>* | 2     | 0.05        | 0.15 → *(2+1) * 0.05*                    |
    | *k<sub>4</sub>* | 1     | 0.10        | 0.20 → *(1+1) * 0.10*                    |
    | *k<sub>5</sub>* | 2     | 0.20        | 0.60 → *(2+1) * 0.20*                    |
    | *d<sub>0</sub>* | 2     | 0.05        | 0.15 → *(2+1) * 0.05*                    |
    | *d<sub>1</sub>* | 2     | 0.10        | 0.30 → *(2+1) * 0.05*                    |
    | *d<sub>2</sub>* | 3     | 0.05        | 0.20 → *(3+1) * 0.05*                    |
    | *d<sub>3</sub>* | 3     | 0.05        | 0.20 → *(3+1) * 0.05*                    |
    | *d<sub>4</sub>* | 3     | 0.05        | 0.20 → *(3+1) * 0.05*                    |
    | *d<sub>5</sub>* | 3     | 0.10        | 0.40 → *(3+1) * 0.10*                    |
    | total           |       |             | 2.80                                     |

### Optimal Substructure
- If an optimal binary search tree *T* has a subtree *T’* containing keys *k<sub>i</sub>, …, k<sub>j</sub>*, then this subtree *T’* must be optimal as well for the subproblem with keys *k<sub>i</sub>*, …, *k<sub>j</sub>* and dummy keys *d<sub>i-1</sub>*, …, *d<sub>j</sub>*
<br><img src="Week 9\Optimal_Substructure.png" width="300px" />

### Overlapping Subproblem: Recurrence (重疊子問題)
- *e[i, j]* : expected cost of searching an optimal binary search tree containing the keys *k<sub>i</sub>*, …, *k<sub>j</sub>*
    - Want to find *e[1, n]*
    - *e[i, i-1]* = *q<sub>i-1</sub>* (only the dummy key *d<sub>i-1</sub>*)
- If *k<sub>r</sub>* (*i ≤ r ≤ j*) is the root of an optimal subtree containing keys *k<sub>i</sub>*, …, *k<sub>j</sub>* and let *w(i, j)* = <img src="Week 9\overlapping.png" width="90px" />, then<br>
*e[i, j] = p<sub>r</sub> + (e[i, r-1] + **w(i, r-1)**) + (e[r+1, j] + **w(r+1, j)**)*<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; = *r[i, r-1] + e[r+1, j] + **w(i, j)***