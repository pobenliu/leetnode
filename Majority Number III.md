# Majority Number III

 Majority Number III ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/majority-number-iii/) )

```
Description
Given an array of integers and a number k, 
the majority number is the number that occurs more than 1/k of the size of the array.
Find it.

Notice
There is only one majority number in the array.

Example
Given [3,1,2,3,2,3,3,4,4,4] and k=3, return 3.

Challenge 
O(n) time and O(k) extra space
```



### 解题思路

在 `HashMap<key, value>` 中维护 `k - 1` 个 `candidate` ，`key` 为数字， `value` 为其出现次数。

- 在 `candidate` 个数小于 `k` 时，将数字及其出现次数依次存入 `HashMap` 或者进行更新。
- 在 `candidate` 个数大于等于 `k` 时，将 `k - 1` 个数的出现次数减 `1` ，并记录次数为 `0` 的数字，将其从 `HashMap` 删除。
- 完成它上述步骤后，在所有数字中，统计出现次数最多的数字，即为结果。

HashMap 的遍历方法

- 如果只需要访问 key

```java
Map<String, Object> map = ...;

for (String key : map.keySet()) {
    // ...
}
```

- 如果只需要访问 value

```java
for (Object value : map.values()) {
    // ...
}
```

- 如果 key 和 value 都需要访问

```java
for (Map.Entry<String, Object> entry : map.entrySet()) {
    String key = entry.getKey();
    Object value = entry.getValue();
    // ...
}
```

##### 易错点

> 1. 当 HashMap 中元素达到 `k` 个时，需要将所有的 `value` 值减一，此时不能直接删除对应的 `key`，推测和 HashMap 的遍历机制有关。需要记录 `value` 为零的 `key`，接下来删除。

Java 实现

```java
public class Solution {
    /**
     * @param nums: A list of integers
     * @param k: As described
     * @return: The majority number
     */
    public int majorityNumber(ArrayList<Integer> nums, int k) {
        if (nums == null || nums.size() == 0) {
            return -1;
        }
        
        // count at most k keys
        HashMap<Integer, Integer> counters = new HashMap<Integer, Integer>();
        for (Integer i : nums) {
            if (!counters.containsKey(i)) {
                counters.put(i, 1);
            } else {
                counters.put(i, counters.get(i) + 1);
            }
            
            if (counters.size() >= k) {
                removeKey(counters);
            }
        }
        
        // corner case
        if (counters.size() == 0) {
            return Integer.MIN_VALUE;
        }
        
        // recalculate counters
        for (Integer i : counters.keySet()) {
            counters.put(i, 0);
        }
        for (Integer i : nums) {
            if (counters.containsKey(i)) {
                counters.put(i, counters.get(i) + 1);
            }
        }
        
        // find the max key
        int maxCounter = 0, maxKey = 0;
        for (Integer i : counters.keySet()) {
            if (counters.get(i) > maxCounter) {
                maxCounter = counters.get(i);
                maxKey = i;
            }
        }
        
        return maxKey;
    }
    
    private void removeKey (HashMap<Integer, Integer> counters) {
        Set<Integer> keySet = counters.keySet();
        List<Integer> removeList = new ArrayList<>();
        for (Integer key : keySet) {
            counters.put(key, counters.get(key) - 1);
            if (counters.get(key) == 0) {
                removeList.add(key);
            }
        }
        for (Integer key : removeList) {
            counters.remove(key);
        }
    }
}
```



### 参考

1. [Majority Number III | 九章算法](http://www.jiuzhang.com/solutions/majority-number-iii/)
2. [Iterate through a HashMap [duplicate] | stackoverflow](http://stackoverflow.com/questions/1066589/iterate-through-a-hashmap)