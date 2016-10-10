#  Remove Duplicates from Sorted Array

 Remove Duplicates from Sorted Array  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/remove-duplicates-from-sorted-array/#) )

```
Description
Given a sorted array, remove the duplicates in place such that each element appear only once and return the new length.
Do not allocate extra space for another array, you must do this in place with constant memory.

Example
Given input array A = [1,1,2],
Your function should return length = 2, and A is now [1,2].
```

### 解题思路

本题目使用双指针（数组下标），一个指针 `p` 遍历数组，另一个指针 `q` 记录最后一个不重复元素的位置。以图示说明。

```
比如输入数组是 [1, 1, 2, 2, 3, 3, 4]
双指针的初始状态如下，上面的指针 p 记录不重复元素位置，下面指针 q 遍历数组：
|
v                          
1  --> 1 --> 2 --> 2 --> 3 --> 3 --> 4
       ^                   
       |

比较两个指针 p, q 指向元素，如果相等，则 p 不移动， q 继续遍历。
|
v                          
1  --> 1 --> 2 --> 2 --> 3 --> 3 --> 4
             ^                   
             |

如果两个指针指向元素不等，那么将 p 右移以为，将 q 指向元素拷贝至 p 处，然后 q 右移一位。
       |
       v                          
1  --> 2 --> 2 --> 2 --> 3 --> 3 --> 4
                   ^                   
                   |
以此类推即可。              
```

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
        if (nums == null || nums.length == 0) {
            return 0;
        }
        
        int pos = 0;
        int i = 1;
        while (i < nums.length) {
            if (nums[i] == nums[pos]) {
                i++;
            } else {
                pos++;
                nums[pos] = nums[i];
                i++;
            }
        }
        return pos + 1;
    }
}
```

