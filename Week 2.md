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
- Runtime: *T(<sub>n</sub>)* = *c<sub>1</sub>n* + *c<sub>2</sub>(n-1)* + *c<sub>4</sub>(n-1)* + *c<sub>5</sub>*<sub><img src="Week 2\c5.PNG" height="25px"/></sub> + <sub><img src="Week 2\c6.PNG" height="25px"/></sub> + <sub><img src="Week 2\c7.PNG" height="25px"/></sub> + <sub><img src="Week 2\c8.PNG" height="20px"/></sub>

### Exact Analysis of Insertion Sort
- *T(<sub>n</sub>)* = *c<sub>1</sub>n* + *c<sub>2</sub>(n-1)* + *c<sub>4</sub>(n-1)* + *c<sub>5</sub>*<sub><img src="Week 2\c5.PNG" height="25px"/></sub> + <sub><img src="Week 2\c6.PNG" height="25px"/></sub> + <sub><img src="Week 2\c7.PNG" height="25px"/></sub> + <sub><img src="Week 2\c8.PNG" height="20px"/></sub>
- **Best case:** If the input is already sorted, all *t<sub>j</sub>'s* are 1
    - Linear (線性): *T(n) = (C<sub>1</sub> + C<sub>2</sub> + C<sub>4</sub> + C<sub>5</sub> + C<sub>8</sub>)n - (C<sub>2</sub> + C<sub>4</sub> + C<sub>5</sub> + C<sub>8</sub>)* 
- **Worst case:** If the array is in reverse sorted order, *t<sub>j</sub>* = *j*, ∀ *j*
    - Quadratic (二次方): *T(n) = <sub><img src="Week 2\c5+c6+c7.PNG" height="25px"/></sub>n<sup>2</sup> + (C<sub>1</sub> + C<sub>2</sub> + C<sub>4</sub> + <sub><img src="Week 2\c5-c6-c7.PNG" height="25px"/></sub> + C<sub>8</sub>) - (C<sub>2</sub> + C<sub>4</sub> + C<sub>5</sub> + C<sub>8</sub>)*
- **Exact analysis is often hard and tedious!** (精確的分析通常很難而且乏味)

### Asymptotic Analysis (漸進分析)
- Asymptotic analysis looks at growth of *T(n)* as *n* -> ∞
- θ notation: drop low-order terms and ignore the leading constant (忽略較低次方及常數)
    - E.g.: *T(n) = 8n<sup>3</sup> - 4n<sup>2</sup> + 5n - 2 = θ(n<sup>3</sup>)*
- As *n* grows large, lower-order θ algorithms outperform higher-order ones (隨著 n 的增大，低階 θ 算法優於高階 θ 算法)
    - E.g.: For large inputs, a θ(n<sup>2</sup>) algorithm will run more quickly in the worst case than a θ(n<sup>3</sup>) algorithm
- Asymptotic analysis of insertion sort (插入排序的漸近分析)
    - **Worst case:** Input reverse sorted, **while** loop is executed *j* times each iteration: <sub><img src="Week 2\worst.PNG" height="20px"/></sub>
        - Calculation process: <sub><img src="Week 2\worst_process.PNG" height="30px"/></sub>
    - **Average case:** **While** loop is executed about *j/2* times each iteration: <sub><img src="Week 2\average.PNG" height="25px"/></sub>
        - Calculation process: <sub><img src="Week 2\average_process.PNG" height="30px"/></sub>
- Those who do algorithms **don’t use exact analysis**, **only use asymptotic analysis**

### *O*: Upper Bounding Function
- **Def:** *f(n) = O(g(n))* if ∃ *c* > 0 and *n<sub>0</sub>* > 0 such that ***0 ≤ f(n) ≤ cg(n)*** for all *n ≥ n<sub>0</sub>*
- Intuition: *f(n)* **≤** *g(n)* when we ignore constant multiples and small values of *n*
- How to **verify** *O* (Big-Oh) relationships?
    - *f(n) = O(g(n))* implies that <sub><img src="Week 2\upper_bound.PNG" height="25px"/></sub> for some **c ≥ 0**, if the limit exists
    <img src="Week 2\upper_bound_graph.PNG" weight="550px"/>


### Big-Oh Examples
- **Def:** *f(n) = O(g(n))* if ∃ *c* > 0 and *n<sub>0</sub>* > 0 such that ***0 ≤ f(n) ≤ cg(n)*** for all *n ≥ n<sub>0</sub>*
- Examples:
    - *3n<sup>2</sup> + n = O(n<sup>2</sup>)*?
        - Yes! <sub><img src="Week 2\o1.PNG" height="28px"/></sub>
    - *3n<sup>2</sup> + n = O(n)*?
        - No! <sub><img src="Week 2\o2.PNG" height="25px"/></sub>
    - *3n<sup>2</sup> + n = O(n<sup>3</sup>)*?
        - Yes! <sub><img src="Week 2\o3.PNG" height="25px"/></sub>
    - *3n<sup>2</sup> + n ≤ cn<sup>2</sup>*? **(⭐Compulsory exam)**
        - **Take *c* = 4, n<sub>0</sub> = 1**

### *Ω*: Lower Bounding Function
- **Def:** *f(n) = Ω(g(n))* if ∃ *c* > 0 and *n<sub>0</sub>* > 0 such that ***0 ≤ cg(n) ≤ f(n)*** for all *n ≥ n<sub>0</sub>*
- Intuition: *f(n)* **≥** *g(n)* when we ignore constant multiples and small values of *n* 
- How to **verify** *Ω* (Big-Omega) relationships?
    - *f(n) = Ω(g(n))* implies that <sub><img src="Week 2\lower_bound.PNG" height="25px"/></sub> for some **c ≥ 0**, if the limit exists
    <img src="Week 2\lower_bound_graph.PNG" weight="550px"/>

### Big-Omega Examples
- **Def:** *f(n) = Ω(g(n))* if ∃ *c* > 0 and *n<sub>0</sub>* > 0 such that ***0 ≤ cg(n) ≤ f(n)*** for all *n ≥ n<sub>0</sub>*
- Examples:
    - *3n<sup>2</sup> + n = Ω(n<sup>2</sup>)*?
        - Yes! <sub><img src="Week 2\omega1.PNG" height="28px"/></sub>
    - *3n<sup>2</sup> + n = Ω(n)*?
        - Yes! <sub><img src="Week 2\omega2.PNG" height="25px"/></sub>
    - *3n<sup>2</sup> + n = Ω(n<sup>3</sup>)*?
        - No! <sub><img src="Week 2\omega3.PNG" height="25px"/></sub>
    - *3n<sup>2</sup> + n ≥ cn<sup>2</sup>*? **(⭐Compulsory exam)**
        - **Take *c* = 2, n<sub>0</sub> = 1**

### *θ*: Tightly Bounding Function
- **Def:** *f(n) = θ(g(n))* if ∃ *c<sub>1</sub>, c<sub>2</sub>* > 0 and *n<sub>0</sub>* > 0 such that ***0 ≤ c<sub>1</sub>g(n) ≤ f(n) ≤ c<sub>2</sub>g(n)*** for all *n ≥ n<sub>0</sub>*
- Intuition: *f(n)* **=** *g(n)* when we ignore constant multiples and small values of *n* 
- How to **verify** *θ* (Theta) relationships?
    - Show both "Big-Oh" (*O*) and "Big-Omega" (*Ω*) relationships
    - *f(n) = θ(g(n))* implies that <sub><img src="Week 2\tightly_bound.PNG" height="30px"/></sub> for some **c > 0**, if the limit exists
    <img src="Week 2\tightly_bound_graph.PNG" weight="550px"/>

### Theta Examples
- **Def:** *f(n) = θ(g(n))* if ∃ *c<sub>1</sub>, c<sub>2</sub>* > 0 and *n<sub>0</sub>* > 0 such that ***0 ≤ c<sub>1</sub>g(n) ≤ f(n) ≤ c<sub>2</sub>g(n)*** for all *n ≥ n<sub>0</sub>*
- Examples:
    - *3n<sup>2</sup> + n = θ(n<sup>2</sup>)*?
        - Yes! <sub><img src="Week 2\theta1.PNG" height="25px"/></sub>
    - *3n<sup>2</sup> + n = θ(n)*?
        - No! <sub><img src="Week 2\theta2.PNG" height="25px"/></sub>
    - *3n<sup>2</sup> + n = θ(n<sup>3</sup>)*?
        - No! <sub><img src="Week 2\theta3.PNG" height="25px"/></sub>
    - *c<sub>1</sub>n<sup>2</sup> ≤ 3n<sup>2</sup> + n ≤ c<sub>2</sub>n<sup>2</sup>*? **(⭐Compulsory exam)**
        - **Take *c<sub>1</sub>* = 2, *c<sub>2</sub>* = 4, n<sub>0</sub> = 1**

### Algorithms with Asymptotic Notation **(⭐Compulsory exam)**
- "Insertion sort is a *O(n<sup>2</sup>)* algorithm" or "The running time of insertion sort is *O(n<sup>2</sup>)* **=> Correct!**
    - The worst-case running time of insertion sort is *O(n<sup>2</sup>)*
    - For any input of size *n*, the running time is at most *cn<sup>2</sup>*
- "Insertion sort is a *Ω(n)* algorithm" or "The running time of insertion sort is *Ω(n)* **=> Correct!**
    - The best-case running time of insertion sort is *Ω(n)*
    - For any input of size *n*, the running time is at least *cn*
- "Insertion sort is a *θ(n<sup>2</sup>)* algorithm" or "The running time of insertion sort is *θ(n<sup>2</sup>)* **=> Wrong!**
    - For a sorted input, insertion sort runs in *θ(n)*
> The teacher suggests that you only use **Big-Oh** when writing a master's thesis or writing paper