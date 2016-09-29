# N-Queens

 N-Queens  ( [leetcode]()  [lintcode]() )

```
Description
The n-queens puzzle is the problem of placing n queens on an n×n chessboard such that no two queens attack each other.
Given an integer n, return all distinct solutions to the n-queens puzzle.
Each solution contains a distinct board configuration of the n-queens' placement, 
where 'Q' and '.' both indicate a queen and an empty space respectively.

Example
There exist two distinct solutions to the 4-queens puzzle:
[
  // Solution 1
  [".Q..",
   "...Q",
   "Q...",
   "..Q."
  ],
  // Solution 2
  ["..Q.",
   "Q...",
   "...Q",
   ".Q.."
  ]
]

Challenge 
Can you do it without recursion?
```

### 解题思路

在国际象棋中，皇后是战斗力最强的一个棋子，可以横、竖、斜走，吃子和走法相同。

本题目的问题是如何在一个 `n * n` 的棋盘上面摆放 `n` 个皇后，使得任何一个皇后都无法直接吃掉其他的皇后？根据国际象棋的规则，要达到目的，任意两个皇后都不能处在同一行、同一列、同一斜线上面。

以 `4 * 4` 棋盘为例，将解决方案中每一行皇后所在列放入一维数组中，两个解决方案分别为 `[2, 4, 1, 3]` 和 `[3, 1, 4, 2]` ，数组索引表示皇后所在行，数组元素表示皇后所在列，不难发现，这其实是 `[1, 2, 3, 4]` 全排列的一部分。所以问题可以转化为①求解所有全排列；②判断全排列是否满足规则要求。

全排列问题使用 `DFS` 搜索即可，我们来看一下如何判断摆放位置是否符合要求。

- 一、不能在同一行，考虑到一维数组索引表示行，所以这个条件是必然满足的；
- 二、不能在同一列，需要将已存入数组的每一个元素值与即将存入的元素比较，两者不同时才可以存入；
- 三、不能在同一斜线，分为两种，左上到右下，右上到左下，如下图所示，两种情况下“斜率”的绝对值都是 `1` ，以此作为判断依据。（注：这里的斜率并不是在常规的笛卡尔坐标系）

|   1, 1   | **1, 2** | **1, 3** | **1, 4** |
| :------: | :------: | :------: | :------: |
| **2, 1** | **2, 2** | **2, 3** | **2, 4** |
| **3, 1** | **3, 2** | **3, 3** | **3, 4** |
| **4, 1** | **4, 2** | **4, 3** | **4, 4** |



Java 实现

```java
class Solution {
    /**
     * Get all distinct N-Queen solutions
     * @param n: The number of queens
     * @return: All distinct solutions
     * For example, A string '...Q' shows a queen on forth position
     */
    ArrayList<ArrayList<String>> solveNQueens(int n) {
        ArrayList<ArrayList<String>> result = new ArrayList<>();
        if (n <= 0) {
            return result;
        }
        
        search(n, new ArrayList<Integer>(), result);
        return result;
    }
    
    private void search (int n,
                        ArrayList<Integer> cols,
                        ArrayList<ArrayList<String>> result) {
        if (cols.size() == n) {
            result.add(drawChessboard(cols));
            return;
        }          
        // cols: index i is row-coordinate && cols.get(i) is column-coordinate
        // for col : the coordinate is (cols.size(), col)
        // col: [0...n-1] form a permutation of column
        for (int col = 0; col < n; col++) {
            if (!isValid(cols, col)) {
                continue;
            }
            cols.add(col);
            search(n, cols, result);
            cols.remove(cols.size() - 1);
        }
    }
    
    private ArrayList<String> drawChessboard (ArrayList<Integer> cols) {
        ArrayList<String> chessboard = new ArrayList<>();
        for (int i = 0; i < cols.size(); i++) {
            StringBuilder cb = new StringBuilder();
            for (int j = 0; j < cols.size(); j++) {
                if (j == cols.get(i)) {
                    cb.append('Q');
                } else {
                    cb.append('.');
                }
            }
            chessboard.add(cb.toString());
        }
        return chessboard;
    }
    
    private boolean isValid (ArrayList<Integer> cols, int col) {
        int row = cols.size();
        for (int i = 0; i < row; i++) {
            // coodinate of the point now is [i, cols.get(i)] , compare with point [row, col]
            // same column
            if (cols.get(i) == col) {
                return false;
            }
            // left-top to right-bottom && right-top to left-bottom
            if (Math.abs(i - row) == Math.abs(cols.get(i) - col)) {
                return false;
            }
        }
        return true;
    }
    
}
```



### 参考

1. [ N-Queens | 九章算法](http://www.jiuzhang.com/solutions/n-queens/)
2. [N-Queens leetcode java | 爱做饭的小莹子](http://www.cnblogs.com/springfor/p/3870944.html)