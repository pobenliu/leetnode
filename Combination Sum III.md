# Combination Sum III

Combination Sum III  ( [leetcode](https://leetcode.com/problems/combination-sum-iii/) )

```
Description
Find all possible combinations of k numbers that add up to a number n, 
given that only numbers from 1 to 9 can be used and each combination should be a unique set of numbers.

Example 1:
Input: k = 3, n = 7
Output:
[[1,2,4]]

Example 2:
Input: k = 3, n = 9
Output:
[[1,2,6], [1,3,5], [2,3,4]]
```

### 解题思路

这道题相当于 Combination Sum II 的 follow up，在其基础上还需要考虑子集中的和是否等于目标值。

Java 实现

```java
public class Solution {
    public List<List<Integer>> combinationSum3(int k, int n) {
        List<List<Integer>> result = new ArrayList<List<Integer>>();
        if (n < 1 || k < 1) {
            return result;
        }
        List<Integer> path = new ArrayList<Integer>();
        dfs(result, path, n, k, 1);
        return result;
    }
    
    private void dfs(List<List<Integer>> result, List<Integer> path,
                        int n, int k, int pos) {
        if (path.size() == k && n == 0) {
            result.add(new ArrayList<Integer>(path));
            return;
        } else if (path.size() == k && n != 0) {
            return;
        }
        for (int i = pos; i <= 9; i++) {
            if (i > n) {
                break;
            }
            path.add(i);
            dfs(result, path, n - i, k, i + 1);
            path.remove(path.size() - 1);
        }
    }
}
```

