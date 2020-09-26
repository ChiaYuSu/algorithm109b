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
