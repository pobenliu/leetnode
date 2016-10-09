#  Find Peak Element

Find Peak Element  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/find-peak-element/#) )

```
There is an integer array which has the following features:
- The numbers in adjacent positions are different.
- A[0] < A[1] && A[A.length - 2] > A[A.length - 1].

We define a position P is a peek if:
A[P] > A[P-1] && A[P] > A[P+1]
Find a peak element in this array. Return the index of the peak.

Notice
The array may contains multiple peeks, find any of them.

Example
Given [1, 2, 1, 3, 4, 5, 7, 6]
Return index 1 (which is number 2) or 6 (which is number 7)

Challenge
Time complexity O(logN)
```

### 解题思路

第一个数和最后一个数都小于其相邻数，所以数组一定存在峰值。考虑使用二分法，取中间值后有以下几种情况：

- 中间值比其右边数小，说明其处在上升沿中，峰值在其右侧， `start = mid` 。
- 中间值比其左边数小，说明其处在下降沿中，峰值在其左侧， `end = mid` 。
- 中间值比两边的数都大，是峰值，直接返回即可。

##### 算法复杂度

- 时间复杂度： `O(log n)` 。
- 空间复杂度： `O(1)` 。

Java 实现：

```java
class Solution {
    /**
     * @param A: An integers array.
     * @return: return any of peek positions.
     */
    public int findPeak(int[] A) {
        // 1.答案在之间，2.不会出界
        int start = 1; 
        int end = A.length-2;  
        while(start + 1 <  end) {
            int mid = (start + end) / 2;
            if(A[mid] < A[mid - 1]) {
                end = mid;
            } else if(A[mid] < A[mid + 1]) {
                start = mid;
            } else {
                end = mid;
            }
        }
        if(A[start] < A[end]) {
            return end;
        } else { 
            return start;
        }
    }
}
```

另一种实现，也通过了测试：

```java
class Solution {
    /**
     * @param A: An integers array.
     * @return: return any of peek positions.
     */
    public int findPeak(int[] A) {
        if (A.length < 3) {
            return -1;
        }
        
        int start = 0;
        int end = A.length - 1;
        int mid;
        while (start + 1 < end) {
            mid = start + (end - start) / 2;
            if (A[mid] < A[mid + 1]) {
                start = mid;
            } else if (A[mid] < A[mid - 1]) {
                end = mid;
            } else {
                return mid;
            }
        }
        return -1;
    }
}

```

### 参考

1. [Find Peak Element | 九章算法](http://www.jiuzhang.com/solutions/find-peak-element/)