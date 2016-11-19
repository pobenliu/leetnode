# Combination Sum

 Combination Sum  ( [leetcode](https://leetcode.com/problems/combination-sum/)  [lintcode]() )

```
Description
Given a set of candidate numbers (C) and a target number (T), 
find all unique combinations in C where the candidate numbers sums to T.
The same repeated number may be chosen from C unlimited number of times.

For example, given candidate set 2,3,6,7 and target 7, 
A solution set is: 
[7] 
[2, 2, 3] 

Notice
All numbers (including target) will be positive integers.
Elements in a combination (a1, a2, … , ak) must be in non-descending order. (ie, a1 ≤ a2 ≤ … ≤ ak).
The solution set must not contain duplicate combinations.

Example
given candidate set 2,3,6,7 and target 7, 
A solution set is: 
[7] 
[2, 2, 3] 
```

### 解题思路

本题与 Subsets 不同之处在于同一个数组元素可以重复使用，以 `[2, 3, 6, 7]` 举例，第一个放入 `list` 中的是首元素 `2` ，下一次递归调用仍将 `2` 放入 `list` 中，直至 `list` 的和大于目标值，然后依次取出 `list` 队尾的 `2` ，开始将 `3` 放入，以此类推。

##### 易错点

> 1. 原始数组中可能有重复元素，所以需要先将原始数组**排序**，在函数中增加**去重处理**。

Java 实现

```java
public class Solution {
    /**
     * @param candidates: A list of integers
     * @param target:An integer
     * @return: A list of lists of integers
     */
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> result = new ArrayList<>();
        if (candidates == null || candidates.length == 0 || target <= 0) {
            return result;
        }
        
        List<Integer> list = new ArrayList<>();
        Arrays.sort(candidates);
        search(candidates, target, 0, list, result);
        return result;
    }
    
    private void search (int[] candidates,
                        int target,
                        int pos,
                        List<Integer> list,
                        List<List<Integer>> result) {
    
        int count = sum(list);
        if (count == target) {
            result.add(new ArrayList<Integer>(list));
            return;
        } else if (count > target) {
            return;
        }
        
        for (int i = pos; i < candidates.length; i++) {
            if (i != pos && candidates[i] == candidates[i - 1]) {
                continue;
            }
            list.add(candidates[i]);
            search(candidates, target, i, list, result);
            list.remove(list.size() - 1);
        }
    }
    
    private int sum(List<Integer> list) {
        if (list == null) {
            return 0;
        }
        
        int count = 0;
        for (int i = 0; i < list.size(); i++) {
            count += list.get(i);
        }
        
        return count;
    }
}
```



九章算法提供的解答将 `list` 求和操作融合在递归函数中，感觉更简洁，供参考。

Java 实现

```java
public class Solution {
    /**
     * @param candidates: A list of integers
     * @param target:An integer
     * @return: A list of lists of integers
     */
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> result = new ArrayList<>();
        if (candidates == null || candidates.length == 0 || target <= 0) {
            return result;
        }
        
        List<Integer> list = new ArrayList<>();
        Arrays.sort(candidates);
        search(candidates, target, 0, list, result);
        return result;
    }
    
    private void search (int[] candidates,
                        int target,
                        int pos,
                        List<Integer> list,
                        List<List<Integer>> result) {
        if (target == 0) {
            result.add(new ArrayList<Integer>(list));
            return;
        }          
        
        for (int i = pos; i < candidates.length; i++) {
            if (candidates[i] > target) {
                break;
            }
            
            if (i != pos && candidates[i] == candidates[i - 1]) {
                continue;
            }
            
            list.add(candidates[i]);
            search(candidates, target - candidates[i], i, list, result);
            list.remove(list.size() - 1);
        }
    }
}
```



### 参考

1. [Combination Sum | 九章算法](http://www.jiuzhang.com/solutions/combination-sum/)