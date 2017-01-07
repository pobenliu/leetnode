#  Digit Counts

 Digit Counts ( [lintcode](http://www.lintcode.com/en/problem/digit-counts/) )

```
Description
Count the number of k's between 0 and n. k can be 0 - 9.

Example
if n = 12, k = 1 in
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12]
we have FIVE 1's (1, 10, 11, 12)
```

### 解题思路

依次判断每个数字含多少个 `k`，注意特殊情况是 `k == 0`。

```java
class Solution {
    /*
     * param k : As description.
     * param n : As description.
     * return: An integer denote the count of digit k in 1..n
     */
    public int digitCounts(int k, int n) {
        // write your code here
        if (k < 0 || k > 9 || n < 0) {
            return 0;
        }
        
        int count = 0;
        for (int i = 0; i <= n; i++) {
            count += helper(k, i);
        }
        return count;
    }
    
    private int helper(int k, int n) {
        if (k == 0 && n == 0) {
            return 1;
        }
        int num = 0;
        while (n != 0) {
            if (n % 10 == k) {
                num++;
            }
            n = n / 10;
        }
        return num;
    }
};

```

