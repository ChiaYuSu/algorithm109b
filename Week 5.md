# Week 5 write-up

## Administrative matters
- Week 6 Quiz 1 postponed to Week 7 (10/30)
- Week 10 Midterm postponed to Week 11 (11/27) 

## Merge Sort and Asymptotic Analysis (合併排序及漸進分析)

### Merge
- **Merge** is the **core part** of Merge Sort
- Pseudo code:
<br><img src="Week 5\merge_pseudo.PNG" width="550px" />

> For the arrays in this book, the array index starts from 1

- Trace code (Demonstrate only for Array A4):
```
0   Merge(Array A4, 1, 1, 2)
1   n1 = 1 - 1 + 1 = 1
2   n2 = 2 - 1 = 1
3   L[1, 2]  R[1, 2]
4   i = 1
5   L[1] = A[1+1-1] = 5
6   j = 1
7   R[1] = A[1+1] = 2
8   L[2] = ∞
9   R[2] = ∞
10  i = 1
11  j = 1
12  k = 1
13  (L[1]=5) >= (R[1]=2), go to line 16
16  A[1] = R[1] = 2
17  j = 2, go to line 12
-----
12  k = 2
13  (L[1]=5) <= (R[2]=∞)
14  A[2] = L[1] = 5
15  i = 2, Array A4 has been sorted
```
- Array *A[5, 2, 4, 6, 1, 3, 2, 6]*
- *q* = (*p* + *r*) // 2
    - *q* = (1 + 8) // 2 = 4
- Pseudo code line 13 **must add the equal sign**, **otherwise** the algorithm will become **unstable**

### Recurrence
- Merge sort is a **recursive algorithm**
- Recurrence for merge sort:
<br><img src="Week 5\mergeSortRecurrence.PNG" width="550px" />

### Recursion Tree for Asymoptotic Analysis (遞歸樹用於漸近分析)
<img src="Week 5\mergeSortRecursionTree.PNG" width="550px" />

- *θ(n lg n)* grows more slowly than *θ(n<sup>2</sup>)*
    - The larger the *n*, the greater the time complexity difference between the two algorithms
- Thus merge sort asymptotically beats insertion sort in the worst case (因此，在最壞的情況下，Merge sort 時間複雜度會比 Insertion sort 來得好)

### Tree height calculation
<img src="Week 5\lgn.PNG" width="550px" />

### Insertion Sort vs. Merge Sort
| | Insertion Sort | Merge Sort |
|-|---|---|
| Approach | Incremental (漸增式) | Divide-and-conquer (分治法) |
| Runtime Complexity | *O(n<sup>2</sup>*) | *θ(n lg n)* |
| Stable? | Yes | Yes |
| In-place? | Yes | No |
| **Note** | Can only be represented by *O* | Can use *O* or *θ* |
- **In-place**: Only a constant # of variables are stored outside the working array (in-place algorithm 是一種使用小的、固定數量的空間來轉換資料的算法，當算法執行時，輸入的資料通常會被要輸出的部份覆蓋掉。簡單來說就是不能開新的記憶體空間存放資料)
- **Stable**: Numbers with the same value in the output array are in the same order as the input array
    - Example:
        - Output: N T<sub>1</sub> U S T<sub>2</sub> is stable
        - Output: N T<sub>2</sub> U S T<sub>1</sub> is nonstable
    - The importance of stable varies by event

### Analyzing Divide-and-Conquer Algorithms
- Recurrence for a divide-and-conquer algorithms:
<br><img src="Week 5\Divide_and_conquer.PNG" width="550px" />

    - *a*: # of subproblems (切成 # 個子問題)
    - *n/b*: size of the subproblems (仔問題的大小)
    - *D(n)*: time to divide the problem of size *n* into subproblems (將大小為 *n* 的問題分解為子問題，所花的時間)
    - *C(n)*: time to combine the subproblem solutions to get the answer for the problem of size *n* (結合 *n* 個子問題，所花的時間)
        - Note: *a* is not necessarily equal to *b*

- Merge sort:
    - *D(n)* = *θ(1)*: compute midpoint of array
    - *C(n)* = *θ(n)*: merging by scanning sorted subarrays **(Merge function)**

### Solving Recurrences
- Three general methods for solving recurrences
    - **Iteration**: Convert the recurrence into a summation by expanding some terms and then bound the summation (暴力法)
    - **Substitution**: Guess a solution and verify it by induction (數學歸納法)
    - **Master Theorem**: If the recurrence has the form ***T(n) = aT(n/b) + f(n)***, then most likely there is a formula that can be applied (主定理) **(⭐Compulsory exam)**
- Two simplifications that won't affect asymptotic analysis (不會影響時間複雜度的簡化)
    - Ignore floors and ceilings
    - Assume base cases are constant, i.e., *T(n) = θ(1)* for small *n*

### Solving Recurrences: Iteration
- Example: *T(n) = 4T(n/2) + n*
<br><img src="Week 5\iteration.PNG" width="550px" />

- Sometimes when master theorem cannot be solved, you can try to use iteration method

### Iteration by Using Recursion Trees
- **Root: computation *(D(n) + C(n))* at top level of recursion**
- Node at level *i*: Subproblem at level *i* in the recursion
- Height of tree: (#levels - 1) in the recursion
- *T(n)* = sum of all nodes in the tree, *T(1) = 1*
- *T(n) = 4T(n/2) + n = n + 2n + 4n + … + 2<sup>lgn</sup>n = θ(n<sup>2</sup>)*
<br><img src="Week 5\iterationTree.PNG" width="550px" />

### Solving Recurrences: Master Theorem **(⭐Compulsory exam)**
- Let **a ≥ 1** and **b > 1** be constants, *f(n)* be an asymptotically positive function, and *T(n)* be defined on nonnegative integers as ***T(n) = aT(n/b) + f(n)***
- Then, *T(n)* can be bounded asymptotically as follows:
    - *T(n) = θ(n<sup>log<sub>b</sub>a</sup>)* if *f(n) = O(n<sup>log<sub>b</sub>a-∈</sup>)* for some constant ∈ > 0
    - *T(n) = θ(n<sup>log<sub>b</sub>a</sup> lg n)* if *f(n) = θ(n<sup>log<sub>b</sub>a</sup>)*
    - *T(n) = θ(f(n))* **if *f(n) = Ω(n <sup>log<sub>b</sub>a+∈</sup>)*** for some constant *∈* > 0 and ***af(n/b) ≦ cf(n)* for some constant *c* < 1** and all sufficiently large *n*
- **Intuition (直覺、直觀): compare *f(n)* with *θ(n<sup>log<sub>b</sub>a</sup>)***
    - Case 1: *f(n)* is polynomially (多項式) smaller than *θ(n<sup>log<sub>b</sub>a</sup>)* 
        - **(*f(n) <  θ(n<sup>log<sub>b</sub>a</sup>)*)**
    - Case 2: *f(n)* is asymptotically (漸進地) equal to *θ(n<sup>log<sub>b</sub>a</sup>)* 
        - **(*f(n) =  θ(n<sup>log<sub>b</sub>a</sup>)*)**
    - Case 3: *f(n)* is polynomially larger than *θ(n<sup>log<sub>b</sub>a</sup>)* 
        - **(*f(n) >  θ(n<sup>log<sub>b</sub>a</sup>)*)**
### Examples of Master Theorem **(⭐Compulsory exam)**
- Eg. 1: *T(n) = 4T(n/2) + n*
    - Compare *f(n) = n* with *θ(n<sup>log<sub>b</sub>a</sup>) = n<sup>2</sup>*: *f(n) = n = O(n<sup>2-∈</sup>)*
    - Case 1 applies: *T(n) = θ(n<sup>log<sub>b</sub>a</sup>) = θ(n<sup>2</sup>)*
- Eg. 2: *T(n) = 2T(n/2) + n*
    - Compare *f(n) = n* with *n<sup>log<sub>b</sub>a</sup> = n*: *f(n) = n = θ(n)*
    - Case 2 applies: *T(n) = θ(n<sup>log<sub>b</sub>a</sup> lg n) = θ(n lg n)*
- Eg. 3: *T(n) = 3T(n/4) + n lg n*
    - Compare *f(n) = n lg n* with *n<sup>log<sub>b</sub>a</sup> = n<sup>0.79</sup>*: *f(n) = n lg n = Ω(n<sup>0.79+∈</sup>)*
    - Case 3 could apply: Need to **check for "regularity" condition** that *af(n/b) ≦ cf(n)*
        - Find c < 1 subject to (滿足) *af(n/b) ≦ cf(n)* for large enough *n*
        - *3(n/4) lg (n/4) ≦ cn lg n* which is true for *c = 3/4*
    - Case 3 applies: *T(n) = θ(f(n)) = θ(n lg n)*

### Examples of Master Theorem (cont'd)
- *T(n) = 2<sup>n</sup>T(n/2) + n<sup>n</sup>*
    - Master theorem can't be applied, **because *a* is not constant**
- *T(n) = 0.5T(n/2) + 1/n*
    - Master theorem can't be applied, **because *a* < 1**
- *T(n) = 64T(n/8) - n<sup>2</sup>log n*
    - Master theorem can't be applied, **because *f(n)* is not positive**
- *T(n) = 4T(n/2) + n<sup>2</sup>lg n*
    - Master theorem can't be applied, **because**
        - *n<sup>log<sub>b</sub>a</sup> = n<sup>2</sup>*, *f(n) = n<sup>2</sup> lg n*, case 3 could apply
        - **Regularity check**: *4(n/2)<sup>2</sup> lg(n/2) ≦ cn<sup>2</sup> lg n*, arrange the formula → *n<sup>2</sup> lg(n/2) ≦ cn<sup>2</sup> lg n*, **because c = 1, so master theorem can't be applied**