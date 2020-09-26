# Week 2 write-up

## Unit 1: Algorithmic Fundamentals
- Course contents:
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
