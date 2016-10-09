#  Search a 2D Matrix II

 Search a 2D Matrix II  ( [leetcode]() [lintcode](http://www.lintcode.com/en/problem/search-a-2d-matrix-ii/#) )

```
Description
Write an efficient algorithm that searches for a value in an m x n matrix, return the occurrence of it.

This matrix has the following properties:
- Integers in each row are sorted from left to right.
- Integers in each column are sorted from up to bottom.
- No duplicate integers in each row or column.

Example
Consider the following matrix:
[
  [1, 3, 5, 7],
  [2, 4, 7, 8],
  [3, 5, 9, 10]
]
Given target = 3, return 2.
```

### 解题思路

矩阵每一行是递增的，每一列是递增的，且没有重复元素。如果从左上角开始遍历，向右和向下两个方向都是递增，无法有效进行判断。如果从左下角开始遍历，

- 若当前值大于目标值，那么这一行的所有数都大于目标值，该行可以舍弃；
- 若当前值小于目标值，那么这一列的所有数都小于目标值，该列可以舍弃；
- 若当前值等于目标值，计数加一，当前行和当前列全部舍弃。

##### 算法复杂度：

- 时间复杂度：每次向上或向右移动一次，从左下角遍历至右上角，移动次数约为 `m + n` ，复杂度为 `O(m + n)` 。
- 空间复杂度：常数个辅助空间，复杂度为 `O(1)` 。

##### 易错点：

> 1. 边界判断的返回值，要根据求解问题的具体含义来定。比如本题中要求的是矩阵中目标值出现的次数，在矩阵为空时，显然目标值个数为 0 ，所以返回 0 即可。不要总是死脑筋的返回 -1 。

Java 实现

```java
public class Solution {
    /**
     * @param matrix: A list of lists of integers
     * @param: A number you want to search in the matrix
     * @return: An integer indicate the occurrence of target in the given matrix
     */
    public int searchMatrix(int[][] matrix, int target) {
        if (matrix == null || matrix.length == 0 ||
            matrix[0] == null || matrix[0].length == 0) {
            return 0;        
        }
        
        int row = matrix.length;
        int col = matrix[0].length;
        int i = row - 1;	// traverse from the left bottom
        int j = 0;
        int count = 0;
      
        while (i >= 0 && j < col) {
            if (matrix[i][j] > target) {
                i--;
            } else if (matrix[i][j] < target) {
                j++;
            } else {
                count++;
                i--;
                j++;
            }
        }
        
        return count;
    }
}

```



### 参考

1. [Search a 2D Matrix II | 九章算法](http://www.jiuzhang.com/solutions/search-a-2d-matrix-ii/)