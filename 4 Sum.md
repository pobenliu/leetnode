# 4 Sum

 4 Sum ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/4sum/) )

```
Description
Given an array S of n integers, 
are there elements a, b, c, and d in S such that a + b + c + d = target?
Find all unique quadruplets in the array which gives the sum of target.

Notice
Elements in a quadruplet (a,b,c,d) must be in non-descending order. (ie, a ≤ b ≤ c ≤ d)
The solution set must not contain duplicate quadruplets.

Example
Given array S = {1 0 -1 0 -2 2}, and target = 0. A solution set is:
(-1, 0, 0, 1)
(-2, -1, 1, 2)
(-2, 0, 0, 2)
```

### 解题思路

本题目可参考 3 Sum 的解题思路，将 4 Sum 降为 3 Sum 问题，再降为 2 Sum 问题，仍是使用双指针实现。

需要注意的是，数组中有重复元素，所以在遍历时要略过重复元素。

Java 实现

```java
public class Solution {
    /**
     * @param numbers : Give an array numbersbers of n integer
     * @param target : you need to find four elements that's sum of target
     * @return : Find all unique quadruplets in the array which gives the sum of
     *           zero.
     */
    public ArrayList<ArrayList<Integer>> fourSum(int[] numbers, int target) {
        ArrayList<ArrayList<Integer>> results = new ArrayList<ArrayList<Integer>>();
        if (numbers == null || numbers.length < 4) {
            return results;
        }
        
        int len = numbers.length;
        Arrays.sort(numbers);
        for (int i = 0; i < len - 3; i++) {
            if (i != 0 && numbers[i] == numbers[i - 1]) {
                continue;
            }
            for (int j = i + 1; j < len - 2; j++) {
                if (j != i + 1 && numbers[j] == numbers[j - 1]) {
                    continue;
                }
                
                int left = j + 1;
                int right = len - 1;
                while (left < right) {
                    int sum = numbers[left] + numbers[right] + numbers[i] + numbers[j];
                    if (sum == target) {
                        ArrayList<Integer> tmp = new ArrayList<Integer>();
                        tmp.add(numbers[i]);
                        tmp.add(numbers[j]);
                        tmp.add(numbers[left]);
                        tmp.add(numbers[right]);
                        results.add(tmp);
                        left++;
                        right--;
                        while (left < right && numbers[left] == numbers[left - 1]) {
                            left++;
                        }
                        while (left < right && numbers[right] == numbers[right + 1]) {
                            right--;
                        }
                    } else if (sum < target) {
                        left++;
                    } else {
                        right--;
                    }
                }
            }
        }
        
        return results;
    }
}
```

考虑推广到更普遍的 k-sum 问题，需要使用递归，将问题逐渐降维。

以下实现还有 bug，尚未弄明白哪里有问题。

```java
public class Solution {
    /**
     * @param numbers : Give an array numbersbers of n integer
     * @param target : you need to find four elements that's sum of target
     * @return : Find all unique quadruplets in the array which gives the sum of
     *           zero.
     */
    public ArrayList<ArrayList<Integer>> fourSum(int[] A, int target) {
        ArrayList<ArrayList<Integer>> ans = new ArrayList<ArrayList<Integer>>();
        /*if (A == null || A.length < 4) {
            return ans;
        }*/
        int n = A.length;
        Arrays.sort(A);
        
        ArrayList<Integer> sol = new ArrayList<Integer>();
        kSum(A, 0, n - 1, target, 4, ans, sol);
        return ans;
    }
    
    private void kSum(int[] A, int start, int end,
                        int target, int k, 
                        ArrayList<ArrayList<Integer>> ans,
                        ArrayList<Integer> sol) {
        if (k < 0) {
            return;
        }
        if (k == 1) {
            for (int i = start; i <= end; i++) {
                if (A[i] == target) {
                    sol.add(target);
                    ans.add(sol);
                    sol.remove(target);
                    return;
                }
            }
        }
        if (k == 2) {
            twoSum(A, start, end, target, ans, sol);
        }
        
        for (int i = start; i <= end - k + 1; i++) {
            if (i > start && A[i] == A[i - 1]) {
                continue;
            }
            sol.add(A[i]);
            kSum(A, i + 1, end, target - A[i], k - 1, ans, sol);
            sol.remove(A[i]);
        }
        
    }
    
    private void twoSum(int[] A, int start, int end,
                        int target,
                        ArrayList<ArrayList<Integer>> ans,
                        ArrayList<Integer> sol) {
        while (start < end) {
            int sum = A[start] + A[end];
            if (sum == target) {
                sol.add(A[start]);
                sol.add(A[end]);
                ans.add(sol);
                sol.remove(A[start]);
                sol.remove(A[end]);
                start++;
                end--;
                while (start < end && A[start] == A[start - 1]) {
                    start++;
                }
                while (start < end && A[end] == A[end + 1]) {
                    end--;
                }
            } else if (sum < target) {
                start++;
            } else {
                end--;
            }
        }                        
    }
}
```



### 参考

1. [[LeetCode] 4Sum |喜刷刷](http://bangbingsyb.blogspot.com/2014/11/leetcode-4sum.html)