# Week 6 write-up
## Administrative matters
- We will have a quiz next week, for Unit 1 and Unit 2
- The exercises are already on Moodle

## Heap Sort
### MAX-HEAPIFY: Maintaining the Heap Property (維護 Heap 的屬性)
- Assume that **subtrees** indexed ***RIGHT(i)*** and ***LEFT(i)*** are **heaps**, but *A[i]* may be smaller than its children (假定左右兩邊的子樹都符合 Heap (下圖中綠色框框)，但 root 節點的值小於其子樹 node 的值)
- MAX-HEAPIFY(*A, i*) will “float down” the value at *A[i]* so that the subtree rooted at *A[i]* becomes a heap
<br><img src="Week 6\MAX-HEAPIFY.PNG" width="550px" />

### MAX-HEAPIFY: Algorithm
- Pseudo code:
<br><img src="Week 6\MAX-HEAPIFY-Pseudo.PNG" width="550px" />

- Trace code:
```

```



