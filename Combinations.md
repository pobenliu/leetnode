#  Combinations

 Combinations  ( [leetcode](https://leetcode.com/problems/combinations/)  [lintcode](http://www.lintcode.com/en/problem/combinations/) )

```
Description
Given two integers n and k, return all possible combinations of k numbers out of 1 ... n.

Example
For example,
If n = 4 and k = 2, a solution is:
[[2,4],[3,4],[2,3],[1,2],[1,3],[1,4]]
```

### 解题思路

Subsets 的变形题，找到所有长度为 `k` 的子集。

Java 实现

```java
public class Solution {
    /**
     * @param n: Given the range of numbers
     * @param k: Given the numbers of combinations
     * @return: All the combinations of k numbers out of 1..n
     */
    public List<List<Integer>> combine(int n, int k) {
		List<List<Integer>> result = new ArrayList<List<Integer>>();
		if (n < 1 || k < 1 || n < k) {
		    return result;
		}
		
		List<Integer> path = new ArrayList<Integer>();
		dfs(result, path, n, k, 1);
		return result;
    }
    
    private void dfs(List<List<Integer>> result, List<Integer> path,
                        int n, int k, int pos) {
        if (path.size() == k) {
            result.add(new ArrayList<Integer>(path));
            return;
        }          
        
        for (int i = pos; i <= n; i++) {
            path.add(i);
            dfs(result, path, n, k, i + 1);
            path.remove(path.size() - 1);
        }
    }
}
```

