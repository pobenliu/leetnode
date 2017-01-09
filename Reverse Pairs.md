#  Reverse Pairs

 Reverse Pairs  ( [lintcode](http://www.lintcode.com/en/problem/reverse-pairs/) )

```
Description
For an array A, if i < j, and A [i] > A [j], called (A [i], A [j]) is a reverse pair.
return total of reverse pairs in A.

Example
Given A = [2, 4, 1, 3, 5] , (2, 1), (4, 1), (4, 3) are reverse pairs. return 3
```

### 解题思路

使用归并排序的思路，这里需要注意的是怎么对逆序对进行计数。

Java 实现

```java
public class Solution {
    /**
     * @param A an array
     * @return total of reverse pairs
     */
    public long reversePairs(int[] A) {
        // Write your code here
        if (A == null || A.length == 0) {
            return 0;
        }
        return mergeSort(A, 0, A.length - 1);
    }
    
    private long mergeSort(int[] A, int start, int end) {
        if (start >= end) {
            return 0;
        }
        
        long sum = 0;
        int mid = start + (end - start) / 2;
        sum += mergeSort(A, start, mid);
        sum += mergeSort(A, mid + 1, end);
        sum += merge(A, start, mid, end);
        
        return sum;
    }
    
    private long merge(int[] A, int start, int mid, int end) {
        int[] temp = new int[A.length];
        int left = start;
        int right = mid + 1;
        int index = start;
        long sum = 0;
        
        while (left <= mid && right <= end) {
            if (A[left] <= A[right]) {
                temp[index++] = A[left++];
            } else {
                temp[index++] = A[right++];
                sum += mid - left + 1;
            }
        }
        while (left <= mid) {
            temp[index++] = A[left++];
        }
        while (right <= end) {
            temp[index++] = A[right++];
        }
        
        for(int i = start; i <= end; i++) {
            A[i] = temp[i];
        }
        
        return sum;
    }
}
```

