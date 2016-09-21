#  Two Sum

 Two Sum   ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/two-sum/) )

```
Description
Given an array of integers, find two numbers such that they add up to a specific target number.
The function twoSum should return indices of the two numbers such that they add up to the target, where index1 must be less than index2. 
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

**算法复杂度**

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)` 

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
- 使用双指针，分别从 `Pair` 的开头和结尾向中间查找。对两个指针对应元素求和
  - 和小于目标值，说明结尾元素较大，排除掉
  - 和小于目标值，说明开头元素较小，排除掉
  - 和等于目标值，返回其对应的索引

**算法复杂度**

- 时间复杂度：用到了基于比较的排序，所以是 `O(nlogn)` 。
- 空间复杂度：用到了

易错点

> 1. `Pair` 的声明
> 2. 比较运算符的重构

Java 实现

以下代码有 `bug` ，测试例 `[1,0,-1], -1` 无法通过，尚未找到哪里有问题

```java
public class Solution {
    /*
     * @param numbers : An array of Integer
     * @param target : target = numbers[index1] + numbers[index2]
     * @return : [index1 + 1, index2 + 1] (index1 < index2)
     */
    class Pair {
        int index;
        int value;
        public Pair(int index, int value) {
            this.index = index;
            this.value = value;
        }
    }
     
    public int[] twoSum(int[] numbers, int target) {
        int[] results = new int[2];
        if (numbers == null || numbers.length == 0) {
            return results;
        }
        
        int len = numbers.length;
        Pair[] nums = new Pair[len + 1];
        nums[0] = new Pair(0, Integer.MIN_VALUE);
        
        for (int i = 1; i <= len; i++) {
            nums[i] = new Pair(i, numbers[i - 1]);
        }
        
        Arrays.sort(nums, new Comparator<Pair> () {
            public int compare (Pair a, Pair b) {
                return a.value - b.value;
            }
        });
        
        int left = 1;
        int right = len;
        while(left < right) {
            int sum = nums[left].value + nums[right].value;
            if (sum > target) {
                right--;
            } else if (sum < target) {
                left++;
            } else {
                results[0] = Math.min(nums[left].index, nums[right].index);
                results[1] = Math.max(nums[left].index, nums[right].index);
                return results;
            }
        }
        
        return results;
    }
}
```



### 参考

