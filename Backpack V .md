# Backpack V 

Backpack V ()

```
Description
Given n items with size nums[i] which an integer array and all positive numbers. 
An integer target denotes the size of a backpack. 
Find the number of possible fill the backpack.
Each item may only be used once.

Example
Given candidate items [1,2,3,3,7] and target 7,
A solution set is:
[7]
[1, 3, 3]
return 2
```

### 解题思路

可参考题目 Backpack IV 的解释，由于不同元素只能取一次，所以内层循环需要从大到小遍历。

Java 实现

```java
public class Solution {
    /**
     * @param nums an integer array and all positive numbers, no duplicates
     * @param target an integer
     * @return an integer
     */
    public int backPackIV(int[] A, int m) {
		if (A == null || A.length == 0 || m <= 0) {
  			return 0;		
  		}
        int n = A.length;
        // status
        int[] f = new int[m + 1];
        // initialize
        f[0] = 1;

        for(int i = 0; i < n; i++){
            for(int j = m; j >= A[i]; j--){
                f[j] += f[j - A[i]];
            }
        }

        return f[m];
    }
}
```

