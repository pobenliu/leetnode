#  N-Queens II

 N-Queens II  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/n-queens-ii/) )

```
Description
Follow up for N-Queens problem.
Now, instead outputting board configurations, return the total number of distinct solutions.

Example
For n=4, there are 2 distinct solutions.
```



### 解题思路

实现思路参考问题  N-Queens ，不必输出具体结果，只需计算结果数量即可，这里需要一个全局变量。∫

Java 实现

```java
class Solution {
    /**
     * Calculate the total number of distinct N-Queen solutions.
     * @param n: The number of queens.
     * @return: The total number of distinct solutions.
     */
    public int result;
    public int totalNQueens(int n) {
        if (n <= 0) {
            return 0;
        }
        
        result = 0;
        search(n, new ArrayList<Integer>());
        return result;
    }
    
    private void search (int n, ArrayList<Integer> cols) {
        if (cols.size() == n) {
            result++;
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
            search(n, cols);
            cols.remove(cols.size() - 1);
        }
    }
    
    private boolean isValid (ArrayList<Integer> cols, int col) {
        int row = cols.size();
        for (int i = 0; i < row; i++) {
            // same column
            if (cols.get(i) == col) {
                return false;
            }
            // same left-top to right-bottom && left-bottom to right-top
            if (Math.abs(i - row) == Math.abs(cols.get(i) - col)) {
                return false;
            }
        }
        return true;
    }
};
```



### 参考

