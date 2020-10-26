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
---
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
    - Worst (LEFT tree) / Total = 2*K* + 1 / 3*K* + 2 → *limit* = 2/3
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
<br><img src="Week 6\Quicksort-Pseudo.PNG" width="550px" />
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
<br><img src="Week 6\PARTITION-Graphic.PNG" width="200px" />

### Quicksort Runtime Analysis: Best Case
- A divide-and-conquer algorithm
    - *T(n) = T(q-p) + T(r-q) + θ(n)*
    - Depends on the position of *q* in *A[p...r]* (時間複雜度大小會跟 q 的位置有關)

#### Best case
- **Best case**: Perfectly balanced splits -- each partition gives an *n/2*: *n/2* split
    - *T(n) = T(n/2) + T(n/2) + θ(n)*
           *= 2T(n/2) + θ(n)*
    - Time comlplexity: ***θ(n lg n)***

