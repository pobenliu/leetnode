#  Combination Sum II

 Combination Sum II  ( [leetcode]()  [lintcode]() )

```
Description
Given a collection of candidate numbers (C) and a target number (T), 
find all unique combinations in C where the candidate numbers sums to T.
Each number in C may only be used once in the combination.

Notice
- All numbers (including target) will be positive integers.
- Elements in a combination (a1, a2, … , ak) must be in non-descending order. 
  (ie, a1 ≤ a2 ≤ … ≤ ak).
- The solution set must not contain duplicate combinations.

Example
Given candidate set [10,1,6,7,2,1,5] and target 8,
A solution set is:
[
  [1,7],
  [1,2,5],
  [2,6],
  [1,1,6]
]
```

### 解题思路

参考 Combination Sum 的解题思路，区别在于不能重复选取数组元素，所以在搜索时下一层递归的起始位置加一。

Java 实现

```java
public class Solution {
    /**
     * @param num: Given the candidate numbers
     * @param target: Given the target number
     * @return: All the combinations that sum to target
     */
    public List<List<Integer>> combinationSum2(int[] num, int target) {
        List<List<Integer>> result = new ArrayList<List<Integer>>();
        if (num == null || num.length == 0 || target <= 0) {
            //result.add(new ArrayList<Integer>());
            return result;
        }
        
        Arrays.sort(num);
        List<Integer> path = new ArrayList<Integer>();
        dfs(result, path, num, target, 0);
        return result;
    }
    
    private void dfs(List<List<Integer>> result, List<Integer> path,
                        int[] num, int target, int pos) {
        if (target == 0) {
            result.add(new ArrayList<Integer>(path));
        }
        
        for (int i = pos; i < num.length; i++) {
            if (num[i] > target) {
                break;
            }
            if (i != pos && num[i] == num[i - 1]) {
                continue;
            }
            path.add(num[i]);
            dfs(result, path, num, target - num[i], i + 1);
            path.remove(path.size() - 1);
        }
    }
}
```

