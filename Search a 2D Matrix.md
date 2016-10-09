# Search a 2D Matrix

Search a 2D Matrix ( [leetcode]() [lintcode](http://www.lintcode.com/en/problem/search-a-2d-matrix/) )

```
Write an efficient algorithm that searches for a value in an m x n matrix.
This matrix has the following properties:
  - Integers in each row are sorted from left to right.
  - The first integer of each row is greater than the last integer of the previous row.

Example
Consider the following matrix:
[
    [1, 3, 5, 7],
    [10, 11, 16, 20],
    [23, 30, 34, 50]
]
Given target = 3, return true.

Challenge
O(log(n) + log(m)) time
```



### 解题思路

#### 一、两次二分法

考虑到二维数组的性质，先寻找对应的行，然后在行中寻找对应位置。

其中，寻找行的问题就是寻找最后一个小于 target 的值的位置。

##### 算法复杂度：

- 时间复杂度： `O(log m) + O(log n)` 。
- 空间复杂度： `O(1)` 。

Java 实现：

```java
// Binary Search Twice
public class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        if (matrix == null || matrix.length == 0) {
            return false;
        }
        if (matrix[0] == null || matrix[0].length == 0) {
            return false;
        }

        int row = matrix.length;
        int column = matrix[0].length;

        // find the row index, the last number <= target
        int start = 0, end = row - 1;
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (matrix[mid][0] == target) {
                return true;
            } else if (matrix[mid][0] < target) {
                start = mid;
            } else {
                end = mid;
            }
        }
        if (matrix[end][0] <= target) {
            row = end;                    
        } else if (matrix[start][0] <= target) {
            row = start;
        } else {
            return false;
        }

        // find the column index, the number equal to target
        start = 0;
        end = column - 1;
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (matrix[row][mid] == target) {
                return true;
            } else if (matrix[row][mid] < target) {
                start = mid;
            } else {
                end = mid;
            }
        }
        if (matrix[row][start] == target) {
            return true;
        } else if (matrix[row][end] == target) {
            return true;
        }
        return false;
    }
}
```



#### 二、一次二分法

将矩阵的每一行拼接起来，成为一个长为 `m × n` 的排序数组，然后使用二分查找。查找时将位置转换为矩阵坐标即可，此方法更加简洁。

##### 算法复杂度：

- 时间复杂度： `O(log m*n) = O(log m) + O(log n)` 。
- 空间复杂度： `O(1)` 。

Java 实现：

```java
public class Solution {
    /**
     * @param matrix, a list of lists of integers
     * @param target, an integer
     * @return a boolean, indicate whether matrix contains target
     */
    public boolean searchMatrix(int[][] matrix, int target) {
        if (matrix == null || matrix.length == 0 ||
            matrix[0] == null || matrix[0].length == 0) {
            return false;        
        }
        
        int row = matrix.length;
        int col = matrix[0].length;
        int start = 0;
        int end = row * col - 1;
        int mid;
        // put all rows of the matrix into an array
        while (start + 1 < end) {
            mid = start + (end - start) / 2;
            int temp = matrix[mid / col][mid % col];
            if (temp == target) {
                return true;
            } else if (temp < target) {
                start = mid;
            } else {
                end = mid;
            }
        }
        if (matrix[start / col][start % col] == target) {
            return true;
        }
        if (matrix[end / col][end % col] == target) {
            return true;
        }
        return false;
    }
}
```



### 参考

1. [Search a 2D Matrix | 九章算法](http://www.jiuzhang.com/solutions/search-a-2d-matrix/)