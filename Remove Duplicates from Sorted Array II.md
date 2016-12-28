# Remove Duplicates from Sorted Array II

 Remove Duplicates from Sorted Array II  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/remove-duplicates-from-sorted-array-ii/#) )

```
Description
Follow up for "Remove Duplicates":
What if duplicates are allowed at most twice?

For example,
Given sorted array A = [1,1,1,2,2,3],
Your function should return length = 5, and A is now [1,1,2,2,3].
```

### 解题思路

参考题目 Remove Duplicates from Sorted Array 的思路，用指针 `curr` 遍历数组，指针 `prev` 记录最后一个不重复元素的位置。考虑到允许同一个元素出现两次，那么 `curr` 指向元素除了与 `prev` 指向元素比较，还需要和 `prev - 1` 指向元素比较。

考虑排序数组的非降特性，所以 `nums[curr] == nums[prev - 1]` 时，也必然有 `nums[prev] == nums[prev]` ，所以，实际上只需要比较 `nums[curr] 和 nums[prev - 1]` 即可。

##### 算法复杂度

- 时间复杂度：遍历数组，需要 `O(n)` 。
- 空间复杂度： `O(1)` 。

Java 实现

```java
public class Solution {
    /**
     * @param A: a array of integers
     * @return : return an integer
     */
    public int removeDuplicates(int[] nums) {
        if (nums.length < 3) {
            return nums.length;
        }
        int prev = 1;
        int curr = 2;
        
        while (curr < nums.length) {
            if (nums[curr] == nums[prev] && nums[curr] == nums[prev - 1]) {
                curr++;
            } else {
                prev++;
                nums[prev] = nums[curr];
                curr++;
            }
        }
        return prev + 1;
    }
}
```



参考 Remove Duplicates from Sorted Array 题目的解法二，有以下 Java 实现

```java
public class Solution {
    /**
     * @param A: a array of integers
     * @return : return an integer
     */
    public int removeDuplicates(int[] nums) {
        // write your code here
        if (nums.length < 3) {
            return nums.length;
        }
        int prev = 1;      // point to previous
        int curr = 2;      // point to current
        for (; curr < nums.length; curr++) {
            if (/* nums[curr] != nums[prev]  ||*/ nums[curr] != nums[prev - 1]) {
                nums[++prev] = nums[curr];
            }
        }

        return prev + 1;
    }
}
```



九章算法还提供了另一种更通用的解法

```java
public class Solution {
    /**
     * @param A: a array of integers
     * @return : return an integer
     */
     // 该解法较为通用
    public int removeDuplicates(int[] nums) {
        // write your code here
        if(nums == null)
            return 0;
        int cur = 0;
        int i ,j;
        for(i = 0; i < nums.length;){
            int now = nums[i];
            for( j = i; j < nums.length; j++){
                if(nums[j] != now)
                    break;
                if(j-i < 2)     // 可根据题目要求，对重复出现最大次数进行修改。
                    nums[cur++] = now;
            }
            i = j;
        }
        return cur;
    }
}
```

### 参考

1. [Remove Duplicates from Sorted Array II | 九章算法](http://www.jiuzhang.com/solutions/remove-duplicates-from-sorted-array-ii/)