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
- Recurrence: *T(n) ≦ T(2n/3) + θ(1) → T(n) = O(lg n)*
