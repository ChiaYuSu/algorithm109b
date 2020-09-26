# Week 2 write-up

## Unit 1: Algorithmic Fundamentals

### Course contents
- On algorithms (關於演算法)
- Mathematical foundations (數學基礎) 
- Asymptotic notation (漸進符號)
- Growth of functions (增長函數)
- Recurrences (遞迴關係式)

### On Algorithms
- Algorithm: A well-defined procedure for transforming some **input** to a desired **output**.
- Major concerns:
    - Correctness (正確的):
        - Does it **halt (停止)**? 
        - Is it **correct (正確)**? 
        - Is it **stable (穩定性，執行 100 次都是一樣的結果)**? 
    - Efficiency (有效率的):
        - **Time** complexity (時間複雜度)? 
        - **Space** complexity (空間複雜度，Ram 夠不夠用)? 
            - Worst case?
            - Average case?
            - (Best) case?
- Better algorithms?
    - How:
        - **Faster** algorithms?
        - Algorithms with **less space** requirement?
    - Optimality (最佳解):
        - Prove that an algorithm is **best possible** and **optimal**?
        - Establish a **lower bound**?
- Applications?
    - **Everywhere in computing!**

### Example: Traveling Salesman Problem (TSP) (旅行銷售員問題)
- Input: A set of points *P* (cities together with a distance *d(p, q)* between any pair *p, q ∈ P*)
- Output: The shortest circular route that starts and ends at a given point and visits all the points
<img src="Week 2\TSP.PNG" width="550px" />

- **Major concerns**: Correct and efficient algorithms?

### Nearest Neighbor Tour
- Pseudo code (虛擬碼):
<img src="Week 2\Nearest_Neighbor_Tour.PNG" width="550px" />

- Simple to implement and very efficient, but **incorrect!** (簡單粗暴，但不是最有效率的)
<img src="Week 2\Nearest_Neighbor_Tour_2.PNG" width="550px" />

### A Correct, But Inefficient Algorithm
- Pseudo code:
<img src="Week 2\correct_but_inefficient.PNG" width="550px" />

- Correctness?
    - Tries all possible orderings of the points -> Guarantees (保證) to end up with the shortest possible tour
- Efficiency?
    - Tries *n!* possible routes!
        - 120 routes for 5 points
        - 3628800 routes for 10 points
        - When there are 20 nodes, how many combinations are there?
- No known efficient, correct algorithm for TSP! (目前尚未找到有效率且正確的 TSP 算法)
    - TSP is **"NP-complete (NPC)"** (不存在同時有效率且正確的解)

### Example of Permutations
- 5! = 120 permutations for only 5 points
<img src="Week 2\permutations.PNG" width="550px" />

## Insertion Sort and Asymptotic Analysis (插入排序及漸進分析)

### Sorting
- Input: A sequence of *n* numbers <*a<sub>1</sub>, a<sub>2</sub>, ..., a<sub>n</sub>*>
- Output: A permutation <*a<sub>1</sub>', a<sub>2</sub>', ..., a<sub>n</sub>'*> such that *a<sub>1</sub>'* ≤ *a<sub>2</sub>'* ≤ ... ≤ *a<sub>n</sub>'*
- Example:
    - Input: <8, 6, 9, 7, 5, 2, 3>
    - Output: <2, 3, 5, 6, 7, 8, 9>
- Concerns:
    - **Correct** and **efficient** algorithms?

### Insertion Sort Illustration
<img src="Week 2\insertion_illustration.PNG" width="550px" />

- Question: What is the invariant of this sort? (插入排序法的不變性是什麼)

### Insertion Sort
- Pseudo code:
<img src="Week 2\insertion_pseudo.PNG" width="550px" />

- Graphic explanation:
<img src="Week 2\insertion_graphic.PNG" width="550px" />

- Trace code:
```
1  j = 2
2  j = 2, key = 6 (A[2] = 6)
3  
4  i = 1, j = 2, key = 6
5  i = 1, j = 2, key = 6 (i > 0 is True & A[i] > 6 is True, do loop)
6  i = 1, j = 2, key = 6 (A[1] = 8, A[2] = 8)
7  i = 0, j = 2, key = 6 (go back to line 5 => i > 0 is False & A[i] > 6 is False, jump out the loop)
8  i = 0, j = 2, key = 6 (A[1] = 6, A[2] = 8 => go back to line 1) 
-----
1  j = 3
2  j = 3, key = 9 (A[3] = 9)
3  
4  i = 2, j = 3, key = 9
5  i = 2, j = 3, key = 9 (i > 0 is True & A[i] > 9 is False, don't enter the loop)
6  
7  
8  i = 2, j = 3, key = 9 (A[3] = 9 => go back to line 1)
-----
And so on...
```

### Correctness?
- Loop invariant (迴圈不變性)
    - At the start of each iteration of the for loop of lines 1 to 8, subarray A[1..j-1] consists of the elements originally in A[1..j-1] but in sorted order (在 1 至 8 行的 for 循環的每次迭代開始時，子數組 A[1..j-1] 按順序**排序完成**)
    <img src="Week 2\insertion_correctness.PNG" width="550px" />

### Loop Invariant for Proving Correctness
- We may use **loop invariants** to prove the correctness (數學歸納法證明)
    - **Initialization:** True before the 1st iteration (k = 1 成立)
    - **Maintenance:** If it is true an iteration, it remains true before the next iteration (k = n-1 成立，證明 k = n 也成立)
    - **Termination:** When the loop terminates, the invariant leads to the correctness of the algorithm (k = n 成立)

### Loop Invariant of Insertion Sort
- **Initialization:** *j = 2* => *A[1]* is sorted (k = 1 成立；k = (j = 2) 成立)
    - **Maintenance:** Move *A[j-1], A[j-2], ...* one position to the right until the position for *A[j]* is found (k = n-1 成立，證明 k = n 也成立；k = (j-1) 成立，證明 k = j 也成立)
    - **Termination:** *j = **n+1*** => *A[1...n]* is sorted. Hence the entire array is sorted! (k = n 成立；k = j 成立)

### Exact Analysis of Insertion Sort (精確分析)
- Pseudo code:
<img src="Week 2\insertion_exact_pseudo.PNG" width="550px" />

- The for loop is executed (n-1) + 1 times. (Why?)
    - Because the *for* loop is used for judgment, even if the judgment is False, this line will still be executed
- *t<sub>j</sub>*: # of times the while loop test for value *j* (i.e., i + # of elements that have to be slid right to insert the *j*-th item)
- Line 5 is executed *t<sub>2</sub>* + *t<sub>3</sub>* + ... + *t<sub>n</sub>* times
- Line 6 and Line 7 are executed (*t<sub>2</sub>* - 1) + (*t<sub>3</sub>* - 1) + ... + (*t<sub>n</sub>* - 1) times
- Runtime: *T(<sub>n</sub>)* = *c<sub>1</sub>n* + *c<sub>2</sub>(n-1)* + *c<sub>4</sub>(n-1)* + *c<sub>5</sub><img src="Week 2\c5.PNG" height="30px"/>*