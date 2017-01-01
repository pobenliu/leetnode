#  Kth Smallest Number in Sorted Matrix

 Kth Smallest Number in Sorted Matrix ( [lintcode](http://www.lintcode.com/en/problem/kth-smallest-number-in-sorted-matrix/) )

```
Description
Find the kth smallest number in at row and column sorted matrix.

Example
Given k = 4 and a matrix:
[
  [1 ,5 ,7],
  [3 ,7 ,8],
  [4 ,8 ,9],
]
return 5

Challenge 
Solve it in O(k log n) time where n is the bigger one between row size and column size.
```

### 解题思路

然后依次将每个数组的最小值放入一个最小堆，从最小堆中取出的第 `k` 个数字就是所求结果。

Java 实现

```java
public class Solution {
    /**
     * @param matrix: a matrix of integers
     * @param k: an integer
     * @return: the kth smallest number in the matrix
     */
    class Node {
        int val, row, col;
        public Node (int val, int row, int col) {
            this.val = val;
            this.row = row;
            this.col = col;
        }
    }
    
    public int kthSmallest(int[][] matrix, int k) {
        // write your code here
        if (matrix == null || matrix.length == 0) {
            return -1;
        }
        
        int n = matrix.length;
        Queue<Node> heap = new PriorityQueue<Node>(n, new Comparator<Node>(){
            public int compare(Node a, Node b) {
                return a.val - b.val;
            }
        });
        
        for (int i = 0; i < n; i++) {
            if (matrix[i].length > 0) {
                int row = i;
                int col = 0;
                int val = matrix[row][col];
                heap.add(new Node(val, row, col));
            }
        }
        
        for (int i = 0; i < k; i++) {
            Node temp = heap.poll();
            int val = temp.val;
            int row = temp.row;
            int col = temp.col;
            
            if (i == k - 1) {
                return val;
            }
            if (col < matrix[row].length - 1) {
                col++;
                val = matrix[row][col];
                heap.add(new Node(val, row, col));
            }
        }
        
        return -1;
    }
}
```

