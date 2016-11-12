# Two Sum

 Two Sum   ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/two-sum/) )

```
Description
Given an array of integers, find two numbers such that they add up to a specific target number.
The function twoSum should return indices of the two numbers such that they add up to the target, 
where index1 must be less than index2. 
Please note that your returned answers (both index1 and index2) are NOT zero-based.

Notice
You may assume that each input would have exactly one solution

xample
numbers=[2, 7, 11, 15], target=9
return [1, 2]

Challenge 
Either of the following solutions are acceptable:
O(n) Space, O(nlogn) Time
O(n) Space, O(n) Time
```

### 解题思路

#### 一、哈希表

依次将数组元素及对应索引放入哈希表，并判断哈希表中是否存在目标值与数字元素的差值，找到后返回对应索引即可。

##### 算法复杂度

- 时间复杂度：`O(n)`。
- 空间复杂度：`O(n)`。

##### 易错点

> 1. 题目要求返回的是元素的自然顺序，而非从零开始的数组索引，故最终返回的索引要在数组索引基础上引加一。

Java 实现

```java
public class Solution {
    /*
     * @param numbers : An array of Integer
     * @param target : target = numbers[index1] + numbers[index2]
     * @return : [index1 + 1, index2 + 1] (index1 < index2)
     */
     
    public int[] twoSum(int[] numbers, int target) {
        int[] results = new int[2];
        if (numbers == null || numbers.length == 0) {
            return results;
        }
        
        HashMap<Integer, Integer> map = new HashMap<>();
        
        for (int i = 0; i < numbers.length; i++) {
            if (map.get(numbers[i]) != null) {
                results[0] = map.get(numbers[i]) + 1;
                results[1] = i + 1;
                return results;
            }
            map.put(target - numbers[i], i);
        }
        
        return results;
    }
}
```



#### 二、双指针

- 建立 `Pair` 数据结构，分别存储索引 `i` 及其对应的数字元素 `numbers[i - 1]` 。
- 对 `Pair` 中的数字元素排序，此处需定义比较运算符。
- 使用双指针，分别从 `Pair` 的开头和结尾向中间查找。对两个指针对应元素求和。
  - 和大于目标值，说明结尾元素较大，排除掉。
  - 和小于目标值，说明开头元素较小，排除掉。
  - 和等于目标值，返回其对应的索引。

##### 算法复杂度

- 时间复杂度：使用基于比较的排序，所以是 `O(nlogn)` 。
- 空间复杂度：`O(n)`。

##### 易错点

> 1. `Pair` 结构的声明、赋值。
> 2. 比较运算符的重构。

Java 实现

```java
public class Solution {
    /*
     * @param numbers : An array of Integer
     * @param target : target = numbers[index1] + numbers[index2]
     * @return : [index1 + 1, index2 + 1] (index1 < index2)
     */
    public class Pair {
        int value;
        int index;
        private Pair (int value, int index) {
            this.value = value;
            this.index = index;
        }
    }

    public int[] twoSum(int[] A, int target) {
        int[] ans = new int[2];
        if (A == null || A.length < 2) {
            return ans;
        }
        
        Pair[] nums = new Pair[A.length];
        for (int i = 0; i < A.length; i++) {
            nums[i] = new Pair(A[i], i + 1);
        }
        
        Arrays.sort(nums, new Comparator<Pair>() {
            public int compare (Pair a, Pair b) {
                return a.value - b.value;
            }    
        });
        
        int left = 0, right = A.length - 1;
        while (left < right) {
            int sum = nums[left].value + nums[right].value;
            if (sum > target) {
                right--;
            } else if (sum < target) {
                left++;
            } else {
                ans[0] = Math.min(nums[left].index, nums[right].index);
                ans[1] = Math.max(nums[left].index, nums[right].index);
                return ans;
            }
        }
        return ans;
    }
}
```



### 参考

1. [Two Sum | 九章算法](http://www.jiuzhang.com/solutions/two-sum/)

