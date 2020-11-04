# Week 7 write-up
## Administrative matters
- It’s been too long for everyone to write the quiz. The difficulty of the mid-term exam will be about **1.5 times** that of the quiz, and the amount of questions will be **5 times** that of the quiz, so you must review the handouts when you go home

## Unit 3: Data Structures on Trees
- Course contents:
    - Binary search trees (二元搜尋樹)
    - Red-black trees (紅黑樹)

## Bianry Search Trees (二元搜尋樹)
### Bianry Search Tree
- Bianry-search-tree (BST) property: Let *x* be a node in a BST
    - If *y* is a node in the **left** subtree of *x*, then ***y.key ≤ x.key***
    - If *y* is a node in the **right** subtree of *x*, then ***x.key ≤ y.key***
- Tree construction: Worst case: *O(n<sup>2</sup>)*; average case: *O(n lg n)*, where *n* is the # of nodes (這裡指的是樹的結構的時間複雜度，並非二元搜尋樹的時間複雜度)
- Operations **Search, Minimum, Maximum, Predecessor (前), Successor (後), Insert, Delete** can be performed in *O(h)* time, where *h* is the **height** of the tree
- Binary tree: Worst case: *h = θ(n)*; balanced BST: *h = θ(lg n)*
- Can we guarantee (保證) *h = θ(lg n)*? Balance search trees!
<br><img src="Week 7\BST.PNG" width="550px" />

### Implementation of a Tree Node
- <img src="Week 7\Tree-Structure.PNG" width="550px" />

### Tree Traversal (遍歷)
- Print out all the keys in sorted order in *θ(n)* time
    - Print the root between the values in its left subtree and those in its right subtree (在其左子樹中的值和其右子樹中的值之間打印 root 節點的值)
    - Pseudo code:
    <br><img src="Week 7\Tree-Traversal.PNG" width="550px" />
- Preorder / Postorder
    - Print the root before / after the values in either subtree (只要交換 Pseudo code 2 到 4 行的 code，可達成 3 種 **(Inorder / Preorder / Postorder)** 不同的排序)
- Example:
    - Tree:
    <br><img src="Week 7\Tree-Traversal-Example.PNG" width="200px" />
    - 3 subtree:
        - Infix: 2, 3, 4, 5, 7, 8 (左 → 根 → 右)
        - Prefix: 5, 3, 2, 4, 7, 8 (根 → 左 → 右)
        - Postfix: 2, 4, 3, 8, 7, 5 (左 → 右 → 根)
> Basic data structure questions often asked in interviews

### Tree Search
- Operations **Search** can be performed in *O(h)* time, where *h* is the height of the tree
- **Recursive** Tree Search
<br><img src="Week 7\Tree-Search-Recursive.PNG" width="550px" />
- Trace code:
```
0  Tree-Search(x = 5, k = 6)
1  (x = NIL) is False or (6 = 5) is False
3  (6 < 5) is False, go to line 5
5  Call Tree-Search(x.right = 7, k = 6)
-----
0  Tree-Search(x = 7, k = 6)
1  (x = NIL) is False or (6 = 7) is False
3  (6 < 7) is True
4  Call Tree-Search(x.left = 6, k = 6)
-----
0  Tree-Search(x = 6, k = 6)
1  (x = NIL) is False or (6 = 6) is True
2  return x = 6
```
- **Iterative** Tree Search
<br><img src="Week 7\Tree-Search-Iterative.PNG" width="550px" />
- Trace code:
```
0  Iterative-Tree-Search(x = 5, k = 6)
1  (x != NIL) is True and (6 != 5) is True
2  6 < 5 is False, go to line 4
4  x = 7, go back to line 1
-----
1  (x != NIL) is True and (6 != 7) is True
2  6 < 7 is True
3  x = 6, go back to line 1
-----
1  (x != NIL) is True and (6 != 6) is False, go to line 5
5  return x = 6
```
- Graphic explanation:
<br><img src="Week 7\Tree-Search-Graph.PNG" width="550px" />
> When two programs have the same number of lines, **iterative execution efficiency is better than recursive**

### Tree Successor
- **Successor** of a node *x*: a node y **with the smallest key such that *key[y] ≥ key[x]***
- **Ancestor** of a node *x*: is any node on the unique path from root to *x*, including *x*
- Pseudo code:
<br><img src="Week 7\Tree-Succesor.PNG" width="550px" />
- Trace code (x = 5):
```
----- Tree-Successor
0  Tree-Successor(x = 5)
1  (7 != NIL) is True
2  Call Tree-Minimum(x.right = 7)
----- Tree-Minimum
0  Tree-Minimum(x = 7)
1  (6 != NIL) is True
2  x = 6, go back to line 1
----- 
1  (x.left != NIL) is False, go to line 3
3  return x = 6, so 5 successor is 6
```
- Trace code (x = 4):
```
----- Tree-Successor
0  Tree-Successor(x = 4)
1  (x.right != NIL) is False
3  y = x.p = 3
4  (y != NIL) is True and (4 == 4) is True
5  x = 3
6  y = 5, go to line 4
-----
4  (y != NIL) is True and (3 == 7) is False, go to line 7
7  return y = 5, so 4 successor is 5
```
> *y* (5) is the lowest ancestor of *x* (4), whose left child (3) is also an ancestor of *x* (4)
- Graphic explanation:
<br><img src="Week 7\Tree-Succesor-Graph.PNG" width="200px" />

### Tree Insertion
- Insert *z* into tree *T*
- Pseudo code
<br><img src="Week 7\Tree-Insert.PNG" width="550px" />
- Trace code:
```
0  Tree-Insert(Tree T, z = 6)
1   y = NIL
2   x = T.root = 5
3   (x != NIL) is True
4   y = 5
5   (6 < 5) is False, go to line 7
7   x = 7, go back to line 3
-----
3   (x != NIL) is True
4   y = 7
5   (6 < 7) is True
6   x = NIL, go back to line 3
-----
3   (x != NIL) is False, go to line 8
-----
8   z.p = y = 7
9   (y == NIL) is False, go to line 11
-----
11  (6 < 7) is True
12  y.left = z = 6
```
- Graphic explanation:
<br><img src="Week 7\Tree-Insertion-Graph.PNG" width="200px" />

### Deletion in Binary Search Trees
- **Case 1**: The node to be deleted (*z*) has **no children** (直接將 node 拿掉)
- **Case 2**: The node to be deleted (*z*) has **only one child** (直接接上上層 node)
- **Case 3**: The node to be deleted (*z*) has **two children** (較複雜，待會探討)
<br><img src="Week 7\Tree-Deletion.PNG" width="550px" />

### Deleting *z* in a Bianry Search Tree
#### If *z* has one child (直接接上上層)
- Pseudo code:
<br><img src="Week 7\Tree-Deletion-One-Child.PNG" width="250px" />
- Trace code:
```
0  Transplant(Tree T, u = 10, v = 6)
1  (u.p == NIL) is False, go to line 3
3  (u == 10) is True
4  u.p.left = 6, go to line 6
6  (v != NIL) is True
7  v.p = u.p = 12
```
> line 3 - 5 更新上對下的關係、line 6 - 7 更新下對上的關係
- Graphic explanation:
<br><img src="Week 7\Case-2.PNG" width="350px" />

#### If *z* has two children
- If ***z's* successor is *z's* right child**
<br><img src="Week 7\Tree-Deletion-1.PNG" width="550px" />
    - 直接將 *z* 拔掉，把 *z* 的 successor 接上去
- **Otherwise**
<br><img src="Week 7\Tree-Deletion-2.PNG" width="550px" />
    - 把 *z* 的 successor 移到 *z* 的位置

### Tree Deletion
- Pseudo code:
<br><img src="Week 7\Tree-Deletion-3.PNG" width="380px" />
<br><img src="Week 7\Tree-Minimum.PNG" width="250px" />
- Trace code:
```
0   Tree-Delete(Tree T, z)
1   if z.left == NIL (z 的左子樹為 NIL)
2   Call Transplant(Tree T, z, z.right)
3   elif z.right == NIL (z 的右子樹為 NIL)
4   Call Transplant(Tree T, z, z.left)
5   else Call Tree-Minimum(z.right) (找到 z 的 successor)
6   if y.p != z (如果 y 沒有接在 z 的腳上)
7   Call Transplant(Tree T, y, y.right)
8   y.right = z.right (z 右腳 (r) 改成接在 y 右腳)
9   y.right.p = y
10  Call Transplant(Tree T, z, y)
11  y.left = z.left
12  y.left.p = y
```
- Pseudo code annotation:
    - line 2: z = node to be deleted, z.right = replaced node
    - line 4: z = node to be deleted, z.left = replaced node
    - line 7: y = node to be deleted, z.right = replaced node
    - line 8 - 9: 更新 y & r 上對下及下對上的關係
    - line 10: z = node to be deleted, y = replaced node
    - line 11 - 12: 更新 y & l 上對下及下對上的關係
- Graphic explanation:
    - Case A:
    <br><img src="Week 7\Tree-Deletion-Pseudo-CaseA.PNG" width="200px" />
    - Case B:
    <br><img src="Week 7\Tree-Deletion-Pseudo-CaseB.PNG" width="200px" />
    - Case C:
    <br><img src="Week 7\Tree-Deletion-Pseudo-CaseC.PNG" width="350px" />
    - Case D:
    <br><img src="Week 7\Tree-Deletion-Pseudo-CaseD.PNG" width="700px" />

## Red-Black Trees
### Balanced Search Trees: Red-Black Trees
- Add **color** field to nodes of binary trees
- **Red-black tree properties:**
    - Every node is either <font color=#00FFFF>red</font> or black