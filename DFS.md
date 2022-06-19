# 搜索 2. DFS

Created: April 10, 2022 11:24 PM
Tags: 岛屿问题
Type: 岛屿问题

# 1. 岛屿问题

## 1. 求数量 (模版)

### **200. Number of Islands**

**Example 1:**

```
Input: grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
Output: 1
```

```java
class Solution {
    private int m, n;
    private int[][] directions = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
    
    public int numIslands(char[][] grid) {
        if(grid == null || grid[0].length == 0){
            return 0;
        }
        m = grid.length;
        n = grid[0].length;
        int res = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == '1') {
                    dfs(grid, i, j);
                    res++;
                }
            }
        }
        return res;
        
    }
    
    private void dfs(char[][] grid, int r, int c) {
        if (r < 0 || r >= m || c < 0 || c >= n || grid[r][c] == '0') {
            return;
        }
        grid[r][c] = '0';
        for (int[] d: directions) {
            dfs(grid, r + d[0], c + d[1]);
        }
    }
}
```

### [1254. Number of Closed Islands](https://leetcode.com/problems/number-of-closed-islands/)

封闭岛屿的数目, 先把边上的dfs掉, 再求数量!

## 2. 求面积

### [695. Max Area of Island](https://leetcode.com/problems/max-area-of-island/)

注意的一点: 在dfs中, `int area = 1` 而不是初始为0.

### [1020. Number of Enclaves](https://leetcode.com/problems/number-of-enclaves/): 所有岛屿的面积之和.

## 3. 变种

### [1905. Count Sub Islands](https://leetcode.com/problems/count-sub-islands/)

trick: 遍历 `grid2`中的所有岛屿，把那些不可能是子岛的岛屿排除掉，剩下的就是子岛。

```java
for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid1[i][j] == 0 && grid2[i][j] == 1) {
                    dfs(grid2, i, j);
                }
            }
        }
```