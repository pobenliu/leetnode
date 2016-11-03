# Backpack V 

Backpack V ( [lintcode](http://www.lintcode.com/zh-cn/problem/backpack-v/) )

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

背包问题：单次选择+装满可能性总数。

#### 一、二维状态

1. 定义状态：定义 `f[i][j]` 为前 `i` 个数填满大小为 `j` 的背包的方法数，每个数只使用一次。
2. 定义状态转移函数：对于当前状态 `f[i][j]` ，
   - 如果不计入 `A[i - 1]` ，则 `f[i - 1][j]` 保持状态不变；
   - 如果计入 `A[i - 1]` ，需满足条件 `j >= A[i - 1]` ，那么 `f[i][j]` 上一个状态是 `f[i - 1][j - A[i-1]]`。
3. 定义起点：
   - `f[i][0]` 状态为 `1` ，这是根据问题的含义来定义的。
4. 定义终点：最终结果即为 `f[n][m]` 。

##### 算法复杂度

- 时间复杂度：`O(nm)`。
- 空间复杂度：`O(nm)`。

Java 实现

```java
public calss Solution {
  /**
     * @param A an integer array and all positive numbers, no duplicates
     * @param m an integer
     * @return an integer
     */
  public static int backPackV (int[] A, int m) {
  	if (A == null || A.length == 0 || m <= 0) {
  		return 0;		
  	}
  	
  	int n = A.length;
	// status
	int[][] f = new int[n + 1][m + 1];
	// initialize
	for (int i = 0; i <= n; i++) {
		f[i][0] = 1;
	}
	for (int i = 1; i <= m; i++) {
		f[0][i] = 0;
	}
  	
  	for (int i = 1; i <= n; i++) {
  		for (int j = 1; j <= m; j++) {
  			f[i][j] = f[i-1][j];
  			if (j >= A[i-1]) {
  				f[i][j] += f[i-1][j - A[i-1]];
  			}
  		}
  	}
  	return f[n][m];
  }
}  
```

使用滚动数组进行优化，可将空间复杂度降至 `O(m)`。

```java
public calss Solution {
  /**
     * @param A an integer array and all positive numbers, no duplicates
     * @param m an integer
     * @return an integer
     */
  public static int backPackV (int[] A, int m) {
  	if (A == null || A.length == 0 || m <= 0) {
  		return 0;		
  	}
  	
  	int n = A.length;
	// status
	int[][] f = new int[2][m + 1];
	// initialize
	for (int i = 0; i < 2; i++) {
		f[i][0] = 1;
	}
	for (int i = 1; i <= m; i++) {
		f[0][i] = 0;
	}
  	
  	for (int i = 1; i <= n; i++) {
  		f[i % 2][0] = 1;
  		for (int j = 1; j <= m; j++) {
  			f[i % 2][j] = f[(i-1) % 2][j];
  			if (j >= A[i-1]) {
  				f[i % 2][j] += f[(i-1) % 2][j - A[i-1]];
  			}
  		}
  	}
  	return f[n % 2][m];
  }
}  
```



#### 二、一维状态

可参考题目 Backpack IV 的解释，由于不同元素只能取一次，所以内层循环需要从大到小遍历。

##### 算法复杂度

- 时间复杂度：`O(nm)`。
- 空间复杂度：`O(m)`。

Java 实现

```java
public class Solution {
    /**
     * @param A an integer array and all positive numbers, no duplicates
     * @param m an integer
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

### 参考

1. [PPT | 背包问题大汇总 | 九章算法](http://mp.weixin.qq.com/s?__biz=MzA5MzE4MjgyMw==&mid=501971746&idx=1&sn=1080cb70842f6217a4c5dbfbafec309a&scene=20#wechat_redirect)