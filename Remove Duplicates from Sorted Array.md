# Remove Duplicates from Sorted Array

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

本题目使用双指针（数组下标），一个指针 `curr` 遍历数组，另一个指针 `prev` 记录最后一个不重复元素的位置。以图示说明。

```
比如输入数组是 [1, 1, 2, 2, 3, 3, 4]
双指针的初始状态如下，上面的指针 prev 记录不重复元素位置，下面指针 curr 遍历数组：
|
v                          
1  --> 1 --> 2 --> 2 --> 3 --> 3 --> 4
       ^                   
       |

比较两个指针指向元素，如果相等，则 prev 不移动， curr 继续遍历。
|
v                          
1  --> 1 --> 2 --> 2 --> 3 --> 3 --> 4
             ^                   
             |

如果两个指针指向元素不等，那么将 prev 右移一位，将 curr 指向元素拷贝至 prev 处，然后 curr 右移一位。
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

##### 易错点

> 1. 在数组的不同元素之间赋值时，注意查看下标是否越界，谨慎使用 `num[++pos] = nums[i++];` 这种表达。
> 2. 题目最后要求返回的是不同元素的个数，所以最后的下标需要加一。

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
        
        int prev = 0;
        int curr = 1;
        while (curr < nums.length) {
            if (nums[curr] == nums[prev]) {
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

