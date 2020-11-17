# Week 8 write-up
## Administrative matters

## Red-Black Trees
### RB-Tree Deletion
- Pseudo code:
<br><img src="Week 7\RB-Delete.PNG" width="550px" />
- Trace code (case A):
```
0   RB-Delete(Tree T, z)
1   y = z (選定要刪除的節點，並將其設成 y)
2   y-origin-color = y.color (儲存即將刪除節點的顏色)
3   (z.left=T.nil) == (T.nil) is True
4   x = z.right
5   Call RB-Transplant(Tree T, z, z.right=x), go to line 21
21  (y-original-color=BLACK) == BLACK
22  Call RB-Delete-Fixup(Tree T, x) → 後面會講到
```
- Pseudo code annotation:
    - line 5: z = node to be deleted, z.right = replaced node
- Trace code (case B):
```
0   RB-Delete(Tree T, z)
1   y = z (選定要刪除的節點，並將其設成 y)
2   y-origin-color = y.color (儲存即將刪除節點的顏色)
3   (z.left=x) == (T.nil) is False, go to line 6
6   (z.right=T.nil) == (T.nil) is True
7   x = z.left
8   Call RB-Transplant(Tree T, z, z.left), go to line 21
21  (y-original-color=BLACK) == BLACK
22  Call RB-Delete-Fixup(Tree T, x36
) → 後面會講到
```
- Pseudo code annotation:
    - line 8: z = node to be deleted, z.left = replaced node
- Trace code (case C):
```
0   RB-Delete(Tree T, z)
1   y = z (選定要刪除的節點，並將其設成 y)
2   y-origin-color = y.color (儲存即將刪除節點的顏色)
3   (z.left=x) == (T.nil) is False, go to line 6
6   (z.right=y) == (T.nil) is False, go to line 9
9   Call Tree-Minimum(z.right=y)
10  
```