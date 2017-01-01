#  Triangle Count

 Triangle Count ( [lintcode](http://www.lintcode.com/en/problem/triangle-count/) )

```
Description
Given an array of integers, how many three numbers can be found in the array, 
so that we can build an triangle whose three edges length is the three numbers that we find?

Example
Given array S = [3,4,6,7], return 3. They are:
[3,4,6]
[3,6,7]
[4,6,7]

Given array S = [4,4,4,4], return 4. They are:
[4(1),4(2),4(3)]
[4(1),4(2),4(4)]
[4(1),4(3),4(4)]
[4(2),4(3),4(4)]
```

### 解题思路

此题目可视为 Two Sum II 的 follow up，三角形的三条边满足——任意两边之和大于第三边，在将数组排序后 `[…a, b, c, ...]`，不难得出 `b + c > a` 和 `a + c > b` 都是必然成立的，只要判断 `a + b > c` 即可。

易错点：

> 1. 记得先给数组排序。

Java 实现

```java
public class Solution {
    /**
     * @param S: A list of integers
     * @return: An integer
     */
    public int triangleCount(int s[]) {
        // write your code here
        if (s == null || s.length < 3) {
            return 0;
        }
        Arrays.sort(s);
        
        int ans = 0;
        for (int i = 2; i < s.length; i++) {
            int start = 0;
            int end = i - 1;
            while (start < end) {
                int sum = s[start] + s[end];
                if (sum > s[i]) {
                    ans += end - start;
                    end--;
                } else {
                    start++;
                }
            }
        } 
        return ans;
    }
}

```

