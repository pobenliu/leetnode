#  Merge Two Sorted Arrays

 Merge Two Sorted Arrays  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/merge-two-sorted-arrays/#) )

```
Description
Merge two given sorted integer array A and B into a new sorted integer array.

Example
A=[1,2,3,4]
B=[2,4,5,6]
return [1,2,2,3,4,4,5,6]

Challenge 
How can you optimize your algorithm if one array is very large and the other is very small?
```

### 解题思路

没什么特别的，比较、合并即可。需要考虑一个数组元素已全部取出，而另外一个数组还有剩余的情况。

Java 实现：

```java
class Solution {
    /**
     * @param A and B: sorted integer array A and B.
     * @return: A new sorted integer array
     */
    public int[] mergeSortedArray(int[] A, int[] B) {
        if (A == null || A.length == 0) {
            return B;
        }
        if (B == null || B.length == 0) {
            return A;
        }

        int M = A.length;
        int N = B.length;
        int[] C = new int [M + N];
        int i = 0, j = 0, k = 0;
        while (i < M && j < N) {
            if (A[i] <= B[j]) {
                C[k++] = A[i++];
            } else {
                C[k++] = B[j++];
            }
        }
        while (i < M) {
            C[k++] = A[i++];
        }
        while (j < N) {
            C[k++] = B[j++];
        }
        return C;
    }
}
```

