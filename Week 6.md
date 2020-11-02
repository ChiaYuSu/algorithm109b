# Week 6 write-up
## Administrative matters
- We will have a quiz next week, for Unit 1 and Unit 2
- The exercises are already on Moodle

## Heap Sort (堆積排序法)
### MAX-HEAPIFY: Maintaining the Heap Property (維護 Heap 的屬性)
- Assume that **subtrees** indexed ***RIGHT(i)*** and ***LEFT(i)*** are **heaps**, but *A[i]* may be smaller than its children (假定左右兩邊的子樹都符合 Heap (下圖中綠色框框)，但 root 節點的值小於其子樹 node 的值)
- MAX-HEAPIFY(*A, i*) will “float down” the value at *A[i]* so that the subtree rooted at *A[i]* becomes a heap
<br><img src="Week 6\MAX-HEAPIFY.PNG" width="550px" />

### MAX-HEAPIFY: Algorithm
- Pseudo code:
<br><img src="Week 6\MAX-HEAPIFY-Pseudo.PNG" width="550px" />

- Trace code:
```
0   MAX-HEAPIFY(Array, i = 2)
1   l = LEFT(2) = 4
2   r = RIGHT(2) = 5
3   l <= 10 is True and (A[4]=14) > (A[2]=4) is True
4   largest = 4
6   r <= 10 is True and (A[5]=7) > (A[4]=14) is False, go to line 8
8   largest = 4 != 2 is True
9   A[2] = 14, A[4] = 4
10  MAX-HEAPIFY(Array, largest = 4)
-----
0   MAX-HEAPIFY(Array, i = 4)
1   l = LEFT(4) = 8
2   r = RIGHT(4) = 9
3   l <= 10 is True and (A[8]=2) > (A[9]=8) is False, go to line 5
5   largest = 4
6   r <= 10 is True and (A[9]=8) > (A[4]=4) is True
7   largest = 9
8   largest = 9 != 4 is True
9   A[4] = 8, A[9] = 4
```
- Graphic explanation:
<br><img src="Week 6\MAX-HEAPIFY-Graphic.PNG" width="550px" />

### MAX-HEAPIFY: Complexity
- Worst case: last row of binary tree is half empty → children's subtrees have size ≦ *2n/3*
- Recurrence: *T(n) ≦ T(2n/3) + θ(1) → T(n) = **O(lg n)***
- How comes 2/3:
    - Worst (LEFT tree) / Total = (2*K* + 1) / (3*K* + 2) → *limit* = 2/3
<br><img src="Week 6\MAX-HEAPIFY-Worst.PNG" width="550px" />

### Build-MAX-HEAP: Building a Max-Heap
- Intuition: Use MAX-HEAPIFY in a buttom-up manner to convert *A* into a heap
    - Leaves are already heaps, start at parents of leaves, and work upward till the root (從不是葉子節點的最後一個節點開始往回)
    <br><img src="Week 6\BUILD-MAX-HEAP.PNG" width="550px" />

### BUILD-MAX-HEAPIFY: Algorithm
- Pseudo code:
<br><img src="Week 6\BUILD-MAX-HEAP-Pseudo.PNG" width="550px" />
    - `downto 1`: `1` means root index 
- Trace code:
```
0  BUILD-MAX-HEAP(Array)
1  A.heap-size = A.length = 10
2  i = 10/2 = 5
3  Call MAX-HEAPIFY(Array, i = 5), go to line 2
-----
2  i = 4
3  Call MAX-HEAPIFY(Array, i = 4), go to line 2
-----
And so on...
```

### BUILD-MAX-HEAP: Complexity
- Naive (單純的) analysis: ***O(n lg n)* time in total**
    - About *n/2* calls to HEAPIFY
    - Each takes *O(lg n)* time
- Careful analysis: ***O(n)* time in total**
    - Each MAX-HEAPIFY takes *O(h)* time (*h*: tree height)
    - At most ⌈*n*/2<sup>*h*</sup>+1⌉ nodes of height *h* in an *n*-element array

### HEAPSORT: Algorithm
- Composed of:
    - BUILD-MAX-HEAP
    - MAX-HEAPIFY
- Pseudo code:
<br><img src="Week 6\HEAPSORT.PNG" width="550px" />
- Trace code:
```
0  HEAPSORT(Array)
1  Call BUILD-MAX-HEAP(Array) → Make tree conform to HEAP properties
2  i = A.length = 10
3  A[1] = 1, A[10] = 16
4  A.heap-size = 9
5  Call MAX-HEAPIFY(Array, 1) → Make root conform to HEAP properties, go back to line 2
-----
And so on...
```

### HEAPSORT: Complexity
- Pseudo code:
<br><img src="Week 6\HEAPSORT.PNG" width="550px" />
- Time complexity: *O(n lg n)*
- Space complexity: *O(n)* for array, so HEAPSORT is **in-place** algorithm
- Is HEAPSORT stable? 
    - Ans: **NO!**

### Queue vs. Stack
- Queue (佇列): FIFO (First In First Out)
- Stack (堆疊): LIFO (Last In First Out)
<br><img src="Week 6\queue_stack.PNG" width="550px" />

### Priority Queues
- A **priority queue** is a **data structure** on sets of keys; a max-priority queue supports the following operations:
    - INSERT(*S, x*): insert *x* into set *S*
    - MAXIMUM(*S*): return the largest key in *S*
    - EXTRACT-MAX(*S*): return and **remove** the largest key in *S*
    - INCREASE-KEY(*S, x, k*): increase the value of element *x*'s key to the new value *k*
    <br><img src="Week 6\insert_max_extract.PNG" width="550px" />
- These operations can be easily supported using a heap
    - INSERT: **Insert the node at the end** and **fix heap** in ***O(lg n)*** time
    - MAXIMUM: Read the first element in ***O(1)*** time
    - INCREASE-KEY: Traverse (遍歷) a path from the target node toward the root to find a proper place for the new key in ***O(lg n)*** time
    - EXTRACT-MAX: Delete the 1st element, replace it with the last, decrement (遞減) the element counter, then heapify in ***O(lg n)*** time

### Heap: INSERT
- Pseudo code:
<br><img src="Week 6\HEAP-INSERT.PNG" width="550px" />
- Trace code:
```
0  MAX-HEAP-INSERT(Array, insert key)
1  A.heap-size = 11
2  i = A.heap-size = 11
3  i > 1 is True and (A[5]=7) < 15 is True
4  A[11] = A[5] = 7
5  i = 5, go to line 3
-----
3  i > 1 is True and (A[2]=14) < 15 is True
4  A[5] = A[2] = 14
5  i = 2, go to line 3
-----
3  i > 1 is True and (A[2]=15) < 15 is False, go to line 6
6  A[2] = 15
```

- Graphic explanation: 
<br><img src="Week 6\HEAP-INSERT-Graphic.PNG" width="550px" />

### Heap: EXTRACT-MAX
- Pseudo code:
<br><img src="Week 6\HEAP-EXTRACT-MAX.PNG" width="550px" />
- Trace code:
```
0  HEAP-EXTRACT-MAX(Array)
1  (A.heap-size = 10) < 1 is False, go to line 3
3  max = A[1] = 16
4  A[1] = A[10] = 1
5  A.heap-size = 9
6  Call MAX-HEAPIFY(Array, 1)
7  return max = 16
```

## Quick Sort (快速排序法)
### Quicksort
- A **divide-and-conquer** algorithm
    - **Devide**: Partition *A[p...r]* into *A[p...q-1]*; each key in *A[p...q-1]* ≦ each key in *A[q+1...r]*
    - **Conquer**: Recursively sort two subarrays
    - **Combine**: Do nothing; quicksort is an **in-place** algorithm
- Quicksort Pseudo code:
<br><img src="Week 6\QuickSort-Pseudo.PNG" width="550px" />
    - **PARTITION (劃分) is the most important part in QUICKSORT**

### Quicksort: Partition
- Partition *A* into two subarrays *A[j]* ≦ *x* and *A[i]* ≧ *x*
- PARTITION runs in *θ(n)* (linear) time, where *n = r - p + 1*
- Ways to pick *x*: always pick *A[r]*, pick a key at random, pick the median of several keys, etc
- There are several partitioning variants
- Pseudo code:
<br><img src="Week 6\PARTITION-Pseudo.PNG" width="550px" />
- Trace code:
```
0  PARTITION(Array, p=1, r=8)
1  x = A[8] = 4
2  i = p - 1 = 0
3  j = 1
4  (A[1]=2) <= 4 is True
5  i = 1
6  A[1]=A[1]=2, A[1]=A[1]=2, go to line 3
-----
3  j = 2
4  (A[2]=8) <= 4 is False
-----
3  j = 3
4  (A[3]=7) <= 4 is False
-----
3  j = 4
4  (A[4]=1) <= 4 is True
5  i = 2
6  A[2]=A[4]=1, A[4]=A[2]=8, go to line 3
-----
3  j = 5
4  (A[5]=3) <= 4 is True
5  i = 3
6  A[3]=A[5]=3, A[5]=A[3]=7, go to line 3
-----
3  j = 6
4  (A[6]=5) <= 4 is False
-----
3  j = 7
4  (A[7]=6) <= 4 is False
-----
7  A[4]=A[8]=4, A[8]=A[4]=8
8  return i+1=4 → q=4
```
- Graphic explanation:
<br><img src="Week 6\PARTITION-Graphic.PNG" width="350px" />

### Quicksort Runtime Analysis: Best Case
- A divide-and-conquer algorithm
    - *T(n) = T(q-p) + T(r-q) + θ(n)*
    - Depends on the position of *q* in *A[p...r]* (時間複雜度大小會跟 q 的位置有關)

#### Best case
- **Best case**: Perfectly balanced splits -- each partition gives an *n/2* (最佳情況：每一次都切中間): *n/2* split
    - *T(n) = T(n/2) + T(n/2) + θ(n)*
           *= 2T(n/2) + θ(n)*
    - Time comlplexity: ***θ(n lg n)***

#### Worst case
- **Worst case**: Each partition gives a 1:*n*-1 split
    - <img src="Week 6\QuickSort-Worst.PNG" width="550px" />
    - <img src="Week 6\QuickSort-Tree.PNG" width="350px" />
    - Time complexity: ***θ(n<sup>2</sup>)***
- **The worst case occurs when an array is sorted!**
<br><img src="Week 6\QuickSort-Worst-Sorted.PNG" width="350px" />

### Quicksort Runtime Analysis: Balanced Partitioning (平衡劃分)
- Suppose the partitioning algorithm always produces a 9-to-1 proportional split
    - *T(n) = T(9/10n) + T(n/10) + θ(n) = **O(n lg n)***
    - <img src="Week 6\QuickSort-Balanced-Partitioning.PNG" width="550px" /> 

### Quicksort: Average-Case Analysis
- **Intuition**: Some splits will be close to balanced and others imbalanced; good and bad splits will be **randomly** distributed in the recursion tree
- **Observation**: Asymptotically bad run time occurs only when we have many ba splits **in a row**
    - A bad split followed by a good split results in a good partitioning after one extra step! (不好的分割再進行良好的分割有可能導致一步完成良好的分區)
    - Thus, we will still get ***O(n lg n)*** run time

### Randomized Quicksort (隨機快速排序)
- How to modify quicksort to get good average-case
behavior on **all** inputs? (如何修改快速排序以在所有輸入上獲得良好的平均情況行為)
- **Randomization!**
    - Randomly permute (置換) inputs
    - Choose the partitioning element *x* randomly at each iteration
- Randomized partition pseudo code:
<br><img src="Week 6\Randomized-Quicksort-Pseudo.PNG" width="550px" />
- Trace code:
```
0  RANDOMIZED-PARTITION(Array, p, r)
1  p to r randomly select an index
2  exchange the last index element and random select element
3  return Array
```

### Average-Case Analysis
- Input array *A[1...n]*, partition at an index *q* with probability *1/n* → The expected value of *T(n)*:
<br><img src="Week 6\Average-Case.PNG" width="550px" />
- Guess *T(n)* ≤ *an lg n + n* and verity it inductively (數學歸納驗證) → **the average-case is bounded by *O(n lg n)***
- **Practically, quicksort is often 2-3 times faster than merge sort or heap sort**

### Decision-Tree Model for Comparison-Based Sorter
- Comparison-based sorters use only comparisons
between elements to gain order information (Comparison-based sorters 僅比較兩個元素的大小)
- The execution of a sorting algorithm ↔ tracing a simple path from the root to a leaf of the decision tree (排序算法的執行 ↔ 從決策樹的根到葉的簡單路徑)
    - **≤**: go to the **left** branch
    - **>**: go to the **right** branch
- The leaves represent all permutations of input (*n!* leaves) (樹葉代表輸入的所有排列)
    - 6 = 1 × 2 × 3
    <br><img src="Week 6\Decision-Tree.PNG" width="550px" />
    

### Ω(n lg n) Lower Bound for Comparison-Based Sorters
- There must be *n!* leaves in the decision tree
- Worst-case # of comparisons = # edge of the longest path in the tree (tree **height**)
- **Theorem**: Any decision tree that sorts *n* elements ha height *Ω(n lg n)*
    - Let *h* be the height of the tree *T*
    - *T* has ≥ *n!* leaves
    - *T* is binary, so has ≤ 2<sup>*h*</sup> leaves
        - 2<sup>*h*</sup> ≥ n!
        - *h* ≥ *lg n!*
            - *= Ω(n lg n)*
- Thus, any comparison-based sorter takes *Ω(n lg n)* time in **the worst case**
- Merge sort and heap sort are asymptotically optimal **comparison** sorters (Merge sort 和 Heap sort 是比較好的漸近最佳排序演算法)

### Comparisons of Comparison-based Sorters
| Algorithm | Best case | Average case | Worst case | In-place | Stable |
| -- | -- | -- | -- | -- | -- |
| Insertion (插入) | *O(n)* | *O(n<sup>2</sup>)* | *O(n<sup>2</sup>)* | Yes | Yes |
| Merge (合併) | *O(n lg n)* | *O(n lg n)* | *O(n lg n)* | N | Y |
| Heap | *O(n lg n)* | *O(n lg n)* | *O(n lg n)* | Y | N |
| Quicksort | *O(n lg n)* | *O(n lg n)* | *O(n<sup>2</sup>)* | Y | N |

- The time complexity of comparison-based sorters is **at least *nlgn***

## Sorting in Linear Time
### Comparison vs. Non-comparison (非比較排序法)
- A sorter is comparison-based if the only operation on
keys is to compare two keys
    - E.g.: Insertion sort, merge sort, heapsort, quicksort
- The non-comparison-based sorters sort keys by **looking
at the values of individual elements**
    - **Counting sort (計數排序)**: Assume keys are in [1...*k*] and uses array indexing to count the # of elements of each value (Counting sort 會去數元素的數量來進行排序)
    - **Radix sort (基數排序)**: Assume each integer contains *d* digits, and each digit is in *[1...*k*]*
    - **Bucket sort (桶排序)**: Sort data into buckets and then merge across buckets. Require information for input **distribution**

### Counting Sort: A Non-comparison-Based Sorter
- **Requirement**: Input integers are in known range *[1...k]*
- **Idea**: For each *x*, find # of elements ≤ *x* (say *m*, excluding *x*) and put *x* in the *(m + 1)st* slot
- Runs in *O(n+k)* time, but needs extra *O(n+k)* space → Means **Not in-place algorithm**
- Example: ***A: input; B: output; C: working array***
- Graphic explanation:
<br><img src="Week 6\Counting-Sort.PNG" width="550px" />