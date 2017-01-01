#  Kth Smallest Sum In Two Sorted Arrays

 Kth Smallest Sum In Two Sorted Arrays ( [lintcode](http://www.lintcode.com/en/problem/kth-smallest-sum-in-two-sorted-arrays/) )

```
Description
Given two integer arrays sorted in ascending order and an integer k. 
Define sum = a + b, where a is an element from the first array and b is an element from the second one. 
Find the kth smallest sum out of all possible sums.

Example
Given [1, 7, 11] and [2, 4, 6].
For k = 3, return 7.
For k = 4, return 9.
For k = 8, return 15.

Challenge 
Do it in either of the following time complexity:
1. O(k log min(n, m, k)). where n is the size of A, and m is the size of B.
2. O( (m + n) log maxValue). where maxValue is the max number in A and B.
```

### 解题思路

考虑到之前做过的题目 Kth Smallest Number in Sorted Matrix，我们可以把这道题转化一下。使用两个指针 `x` 和 `y` 分别遍历两个数组，每次将其中之一加一，然后求和加入最小堆。从堆中取出的第 `k` 个元素就是第 `k` 大的和。

一些需要注意的地方，每次从堆中取出一个元素，然后分别将 `x` 和 `y` 移位，将对应和加入堆。考虑到可能会出现重复，所以使用一个 `HashSet` 来记录求过的和。

也可以使用一个二维数组 `boolean[][] visited` 记录是否已经求和，但是 lintcode 提交出错了。

Java 实现

```java
public class Solution {
    /**
     * @param A an integer arrays sorted in ascending order
     * @param B an integer arrays sorted in ascending order
     * @param k an integer
     * @return an integer
     */
    class Node {
        public int x, y, val;
        public Node (int x, int y, int val) {
            this.x = x;
            this.y = y;
            this.val = val;
        }
    }
    
    public int kthSmallestSum(int[] A, int[] B, int k) {
        // Write your code here
        if (A == null || A.length == 0 ||
            B == null || B.length == 0 ||
            A.length * B.length < k) {
            return -1;        
        } 
        
        PriorityQueue<Node> heap = new PriorityQueue<Node>(k, new Comparator<Node>(){
            public int compare(Node a, Node b) {
                return a.val - b.val;
            }
        });
        HashSet<String> set = new HashSet<String>();
        
        heap.offer(new Node(0, 0, A[0] + B[0]));
        set.add(0 + "," + 0);
        
        int[] dx = new int[]{0, 1};
        int[] dy = new int[]{1, 0};
        
        for (int i = 0; i < k - 1; i++) {
            Node temp = heap.poll();
            for (int j = 0; j < 2; j++) {
                int x = temp.x + dx[j];
                int y = temp.y + dy[j];
                
                if (isValid(x, y, A, B, set)) {
                    int val = A[x] + B[y];
                    heap.offer(new Node(x, y, val));
                    set.add(x + "," + y);
                }
            }
        }
        return heap.peek().val;
    }
    
    private boolean isValid(int x, int y, int[] A, int[] B, HashSet<String> set) {
        if (x < A.length && y < B.length && !set.contains(x + "," + y)) {
            return true;
        }
        return false;
    }
}
```



### 参考

1. [Kth Smallest Sum In Two Sorted Arrays 465 | LintCode题解](https://zhengyang2015.gitbooks.io/lintcode/content/kth_smallest_sum_in_two_sorted_arrays_465.html)
2. [Kth Smallest Sum In Two Sorted Arrays | LintCode & LeetCode 题解分析](https://aaronice.gitbooks.io/lintcode/content/data_structure/kth_smallest_sum_in_two_sorted_arrays.html)

