#  Ugly Number II

 Ugly Number II  ( [lintcode](http://www.lintcode.com/en/problem/ugly-number-ii/) )

```
Description
Ugly number is a number that only have factors 2, 3 and 5.
Design an algorithm to find the nth ugly number. 
The first 10 ugly numbers are 1, 2, 3, 4, 5, 6, 8, 9, 10, 12...

Notice
Note that 1 is typically treated as an ugly number.

Example
If n=9, return 10.
```

### 解题思路

根据丑数的定义依次从小到大生成 `n` 个丑数，用三个指针 `p2, p3, p5` 分别保存乘以 `2, 3, 5` 以后大于当前最大丑数的最小数，三个新的丑数中的最小值即为下一个丑数。

Java 实现

```java
class Solution {
    /**
     * @param n an integer
     * @return the nth prime number as description.
     */
    public int nthUglyNumber(int n) {
        // Write your code here
        if (n <= 0) {
            return 0;
        }
        
        int[] list = new int[n];
        list[0] = 1;
        int p2 = 0, p3 = 0, p5 = 0;
        
        
        for (int i = 1; i < n; i++) {
            while (list[p2] * 2 <= list[i - 1]) {
                p2++;
            }
            while (list[p3] * 3 <= list[i - 1]) {
                p3++;
            }
            while (list[p5] * 5 <= list[i - 1]) {
                p5++;
            }
            
            list[i] = Math.min(list[p2] * 2, Math.min(list[p3] * 3, list[p5] * 5));
        }
        
        return list[n - 1];
    }
    
};

```

