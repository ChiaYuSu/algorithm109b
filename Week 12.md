# Week 12 write-up
## Administrative matters
- There will be a quiz next week, the test scope is **Unit5** and **Unit6**

## Unit 6: Disjoint Sets (不相交集合、互斥集合)
- Course contents:
    - Data structures for disjoint sets

### Disjoint Sets
- A disjoint-set data structure maintains a collection *S = {S<sub>1</sub>, S<sub>2</sub>, ..., S<sub>k</sub>}* of disjoint dynamic sets
    - E.g.:
        - A = {1, 3, 7, 8}
        - B = {4, 5}
        - C = {2}
        - D = {1, 2, 3}
            - A, B, C: disjoint sets
            - A, B, C, D: not disjoint sets
- Each set is identified by a representative, which is some member of the set
- Operations supported:
    - **Make-Set(*x*):** *S<sub>x</sub> = {x}*
        - Create a new set whose only member (and thus representative) is *x*
    - **Union(*x*, *y*):** *S<sub>x</sub> ∪ S<sub>y</sub>*
    - **Find-Set(*x*):** representative of the set containing *x*
- Disjoint sets are useful in a mininum spanning tree algorithm and many other applications

### Example
```
MAKE-SET(1)     {1}
MAKE-SET(2)     {2}
MAKE-SET(3)     {3}
MAKE-SET(4)     {4}
FIND(3)         (returns 3)
FIND(2)         (returns 2)
UNION(1,2)      (representative 1, say) {1,2}
FIND(2)         (returns 1)
FIND(1)         (returns 1)
UNION(3,4)      (representative 4, say) {3,4}
FIND(4)         (returns 4)
FIND(3)         (returns 4)
UNION(1,3)      (representative 4, say) {1,2,3,4}
FIND(2)         (returns 4)
FIND(1)         (returns 4)
FIND(4)         (returns 4)
FIND(3)         (returns 4)
```

### Application: Connected Components
- Pseudo code:
    - Connected-Components
    <br><img src="Week 12\connected_components.PNG" />
    - Same-Component
    <br><img src="Week 12\same_component.PNG" />
- Trace code:
    - Connected-Components
    ```
    1  vertex a - vertex j
    2  Make-Set(a) - Make-Set(j) → {a, b, c, d, e, f, g, h, i}
    3  edge(b, d)
    4  (b != d) is True
    5  {b, d}
    sets → {a}, {b, d}, {c}, {e}, {f}, {g}, {h}, {i}, {j}
    ---
    3  edge(e, g)
    4  (e != g) is True
    5  {e, g}
    sets → {a}, {b, d}, {c}, {e, g}, {f}, {h}, {i}, {j}
    ---
    3  edge(a, c)
    4  (a != c) is True
    5  {a, c}
    sets → {a, c}, {b, d}, {e, g}, {f}, {h}, {i}, {j}
    ---
    3  edge(h, i)
    4  (h != i) is True
    5  {h, i}
    sets → {a, c}, {b, d}, {e, g}, {f}, {h, i}, {j}
    ---
    3  edge(a, b)
    4  (a != b) is True
    5  {a, b, c, d}
    sets → {a, b, c, d}, {e, g}, {f}, {h, i}, {j}
    ---
    3  edge(e, f)
    4  (e != f) is True
    5  {e, f, g}
    sets → {a, b, c, d}, {e, f, g}, {h, i}, {j}
    ---
    3  edge(b, c)
    4  (b != c) is False
    sets → {a, b, c, d}, {e, f, g}, {h, i}, {j}
    ```
    - Same-Component
    ```
    0  Same-Component(a, c)
    1  (a == c) is True
    2  return True
    ---
    0  Same-Component(a, e)
    1  (a == e) is False, go to line 3
    3  return False
    ```
- Graphic explanation:
<br><img src="Week 12\connected_components_graphic.PNG" />

### Linked-List Representation of Disjoint Sets
<img src="Week 12\disjoint_set.PNG" />

- *n*: # of Make-Set operations
- *m*: total # of Make-Set, Find-Set, Union operations
- Operations supported:
    - Make-Set: *O(1)* time (*θ(n)* for *n* Make-Set operations)
    - Find-Set: *O(1)* time
    - Union: Very slow → *O(n<sup>2</sup>)*

### Time Complexity
- *n*: # of Make-Set operations
- *m*: total # of Make-Set, Find-Set, Union operations
- Each **naive** Union takes linear time
    - <img src="Week 12\union_time_complexity.PNG" height="20px"/> for *n-1* Union operations
- Total time for the *m* operations: *O(m + n<sup>2</sup>)*
<br><img src="Week 12\union_time_complexity_2.PNG" />
- **Weight-union heuristic:** Append the smaller list onto the longer
- **Theorem:** Using the weight-union heuristic, a sequence of *m* Make-Set, Union, and Find-Set operations, with *n* Make-Set operations, takes *O(m + **n lg n**)* time