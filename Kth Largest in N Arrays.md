# Kth Largest in N Arrays

Kth Largest in N Arrays 

```
Description
Find K-th largest element in N arrays.

Notice
You can swap elements in the array

Example
In n=2 arrays [[9,3,2,4,7],[1,2,3,4,8]], the 3rd largest element is 7.
In n=2 arrays [[9,3,2,4,8],[1,2,3,4,2]], the 1st largest element is 9, 2nd largest element is 8,
3rd largest element is 7 and etc.
```

### 解题思路

首先将每个一维数组排序，然后依次将每个数组的最大值放入一个最大堆，从最大堆中取出的第 `k` 个数字就是所求结果。题目的关键是在从堆中取出数字后还要将对应一维数组的最大值加入堆，需要新建一个 `Node` 类，来记录数组数值，数组行，数组列。

Java 实现

```java


class Node {
    public int value, row_id, col_id;
    public Node(int _v, int _row, int _col) {
        this.value = _v;
        this.row_id = _row;
        this.col_id = _col;
    }
}

public class Solution {
    /**
     * @param arrays a list of array
     * @param k an integer
     * @return an integer, K-th largest element in N arrays
     */
    public int KthInArrays(int[][] arrays, int k) {
        // Write your code here
        if (arrays == null || arrays.length == 0 ) {
            return -1;
        }
        int m = arrays.length;
        
        PriorityQueue<Node> heap =  new PriorityQueue<Node>(k, new Comparator<Node>(){
            public int compare(Node a, Node b) {
                return b.value - a.value;
            }
        }); 
        
        for (int i = 0; i < m; i++) {
            Arrays.sort(arrays[i]);
            if (arrays[i].length > 0) {
                int row_id = i;
                int col_id = arrays[i].length - 1;
                int value = arrays[i][col_id];
                heap.add(new Node(value, row_id, col_id));
            }
        }
        
        for (int i = 0; i < k; i++) {
            Node temp = heap.poll();
            int row_id = temp.row_id;
            int col_id = temp.col_id;
            int value = temp.value;
            
            if (i == k - 1) {
                return value;
            }
            
            if (col_id > 0) {
                col_id--;
                value = arrays[row_id][col_id];
                heap.add(new Node(value, row_id, col_id));
            }
        }
        
        return -1;
    }
}
```

