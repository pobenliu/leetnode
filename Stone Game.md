# Stone Game 

Stone Game  ( [lintcode]() )

```
Description
There is a stone game.
At the beginning of the game the player picks n piles of stones in a line.
The goal is to merge the stones in one pile observing the following rules:
1. At each step of the game,the player can merge two adjacent piles to a new pile.
2. The score is the number of stones in the new pile.
You are to determine the minimum of the total score.

Example
For [4, 1, 1, 4], in the best solution, the total score is 18:
1. Merge second and third piles => [4, 2, 4], score +2
2. Merge the first two piles => [6, 4]，score +6
3. Merge the last two piles => [10], score +10

Other two examples:
[1, 1, 1, 1] return 8
[4, 4, 5, 9] return 43
```

### 解题思路

#### 一、记忆化搜索

1. 定义状态：`f[i][j]` 表示把第 `i` 个到第 `j` 个石头合并在一起的最小分数值。
2. 定义状态转移函数： 对 `f[i][j]`，可以有任意划分，得到 `f[i][j] = f[i][k] + f[k+1][j] + sum[i][j], i<=k<j`，只需遍历所有取值 k 求最小值即可。其中 `sum[i][j]` 表示第 `i` 个到第 `j` 个石头的总分值。
3. 定义起点：初始化状态转移函数无法涉及的状态 `f[i][i] = 0, 0 <= i < n}` 。
4. 定义终点：为 `f[0][n - 1]`。

Java 实现

```java
public class Solution {
    /**
     * @param A an integer array
     * @return an integer
     */
    public static int stoneGame(int[] A) {
        if (A == null || A.length == 0) {
            return 0;
        }
    
    int n = A.length;
    // status
    int[][] f = new int[n][n];
    int[][] visit = new int[n][n];
    
    // initialize
    for (int i = 0; i < n; i++) {
        f[i][i] = 0;
    }
    
    int[][] sum = new int[n][n];
    for (int i = 0; i < n; i++) {
        sum[i][i] = A[i];
        for (int j = i + 1; j < n; j++) {
            sum[i][j] = sum[i][j - 1] + A[j];
        }
    }
    
    return search(0, n - 1, f, visit, sum);
    }
    
    private static int search (int l, int r, int[][] f, int[][] visit, int[][] sum) {
        if (visit[l][r] == 1) {
            return f[l][r]; 
        }
        
        if (l == r) {
            visit[l][r] = 1;
            return f[l][r];
        } 
        
        f[l][r] = Integer.MAX_VALUE;
        for (int k = l; k < r; k++) {
            f[l][r] = Math.min(
                        f[l][r],
                        search(l, k, f, visit, sum) + search(k+1, r, f, visit, sum) + sum[l][r]
                        );
        }
        visit[l][r] = 1;
        return f[l][r];
    }
}
```



#### 二、动态规划



Java 实现

```java
public class Solution {
    /**
     * @param A an integer array
     * @return an integer
     */
    public static int stoneGame(int[] A) {
        if (A == null || A.length == 0) {
            return 0;
        }
    
        int n = A.length;
        // status
        int[][] f = new int[n][n];
        
        // initialize
        for (int i = 0; i < n; i++) {
            f[i][i] = 0;
        }
        
        int[][] sum = new int[n][n];
        for (int i = 0; i < n; i++) {
            sum[i][i] = A[i];
            for (int j = i + 1; j < n; j++) {
                sum[i][j] = sum[i][j - 1] + A[j];
            }
        }
        
        for (int delta = 1; delta < n; delta++) {
            for (int i = 0; i + delta < n; i++) {
                int j = i + delta;
                f[i][j] = Integer.MAX_VALUE;
                for (int k = i; k < j; k++) {
                    f[i][j] = Math.min(f[i][j], f[i][k] + f[k + 1][j] + sum[i][j]);
                }
            }
        }
        
        return f[0][n - 1];
  }  
}
```



### 参考

1. [Stone Game 476 | LintCode题解](https://zhengyang2015.gitbooks.io/lintcode/content/stone_game_476.html)

