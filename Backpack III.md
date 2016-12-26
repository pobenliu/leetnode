# Backpack III

Backpack III  ( [lintcode](http://www.lintcode.com/zh-cn/problem/backpack-iii/) )

```
Description
Given n kind of items with size Ai and value Vi( each item has an infinite number available) 
and a backpack with size m. 
What's the maximum value can you put into the backpack?

Notice
You cannot divide item into small pieces and the total size of items you choose should smaller or equal to m.

Example
Given 4 items with size [2, 3, 5, 7] and value [1, 5, 2, 4], and a backpack with size 10. 
The maximum value is 15.
```

### 解题思路

背包问题：重复选择+最大价值。

#### 一、二维状态

1. 定义状态：定义 `f[i][j]` 为 `A` 的前 `i` 个数填满体积为 `j` 的背包可得的最大价值，每个数可重复使用。
2. 定义状态转移函数：对于当前状态 `f[i][j]` ，
   - 如果不计入 `A[i - 1]` ，则 `f[i][j]` 对应的上一个状态是 `f[i - 1][j]` ；
   - 如果计入 `A[i - 1]` ，需满足条件 `j >= A[i - 1]` ，那么 `f[i][j]` 上一个状态是 `f[i][j - A[i - 1]]` ；
   - 以上两种情况取和即可。
3. 定义起点：
   - 当 `i != 0` ，`j == 0` 时，如果一个数字都不计入，即取前 `i` 个数字中的零个，那么最大取值为零，所以 `f[i][0] = 0` 。
   - 当 `i == 0` ，`j != 0` 时，前零个元素的和显然无法填满大小为 `j` 的背包， `f[0][j] = 0` 。
   - `f[0][0] = 0` 。
4. 定义终点：最终结果即为 `f[n][m]` 。

##### 算法复杂度

- 时间复杂度：`O(nm)`。
- 空间复杂度：`O(nm)`。

Java 实现

```java
public class Solution {
  public static int backpackIII(int[] A, int[] V, int m) {
  	if (A == null || A.length == 0 ||
  		V == null || V.length == 0 ||
  		m <= 0) {
  		return 0;		
  	}
  	
  	int n = A.length;
  	// status	
  	int[][] f = new int[n + 1][m + 1];
  	for (int i = 1; i <= n; i++) {
  		for (int j = 1; j <= m; j++) {
  			f[i][j] = f[i-1][j];
  			if (j >= A[i-1]) {
  				f[i][j] = Math.max(f[i][j], f[i][j - A[i-1]] + V[i-1]);
  			}
  		}
  	}
  	
  	return f[n][m];
  }
}
```

使用滚动数组优化空间，可使空间复杂度降至 `O(m)`。

```java
public class Solution {
  public static int backpackIII(int[] A, int[] V, int m) {
  	if (A == null || A.length == 0 ||
  		V == null || V.length == 0 ||
  		m <= 0) {
  		return 0;		
  	}
  	
  	int n = A.length;
  	// status	
  	int[][] f = new int[2][m + 1];
  	for (int i = 1; i <= n; i++) {
  		for (int j = 1; j <= m; j++) {
  			f[i % 2][j] = f[(i-1) % 2][j];
  			if (j >= A[i-1]) {
  				f[i % 2][j] = Math.max(f[i % 2][j], f[i % 2][j - A[i-1]] + V[i-1]);
  			}
  		}
  	}
  	
  	return f[n % 2][m];
  }
}
```



#### 二、一维状态

1. 定义状态：定义 `f[j]` 为前 `i` 个数填满大小为 `j` 的背包，最大值是多少。
2. 定义状态转移函数：对于当前状态 `f[j]` ，
   - 如果不计入 `A[i]` ，则 `f[j]` 保持状态不变；
   - 如果计入 `A[i]` ，需满足条件 `j > A[i]` ，那么 `f[j]` 上一个状态是 `f[j - A[i]]` 
   - 对于当前物品 `i`，若 `j` 从小到大的话，很可能在 `j` 之前的 `j - A[i]` 时已经放过第 `i` 件物品了，在 `j` 时再放就是重复放入；若 `j` 从大到小，则 `j` 之前的所有情况都没有更新过，不可能放过第 `i` 件物品，所以不会重复放入。【DH：想得不是很明白...】
3. 定义起点：
   - `f[0]` 状态为 `0` 。
4. 定义终点：最终结果即为 `f[m]` 。

##### 算法复杂度

- 时间复杂度：`O(nm)`。
- 空间复杂度：`O(m)`。

Java 实现

```java
public class Solution {
    /**
     * @param A an integer array
     * @param V an integer array
     * @param m an integer
     * @return an array
     */
    public int backPackIII(int[] A, int[] V, int m) {
	    if (A == null || A.length == 0 ||
  			V == null || V.length == 0 ||
  			m <= 0) {
  			return 0;		
  	    }
        int[] f = new int[m + 1];

        for(int i = 0; i < A.length; i++){
            for(int j = 1; j <= m; j++){
                if (j >= A[i]) {
  				f[j] = Math.max(f[j], f[j - A[i]] + V[i]);
  			   }
            }
        }
        return f[m];
    }
}
```



### 参考

1. [[LintCode] Backpack I II III IV V VI [背包六问]](https://segmentfault.com/a/1190000006325321)
2. [Backpack III 440 | LintCode题解](https://zhengyang2015.gitbooks.io/lintcode/content/backpack_iii_440.html)
3. [PPT | 背包问题大汇总 | 九章算法](http://mp.weixin.qq.com/s?__biz=MzA5MzE4MjgyMw==&mid=501971746&idx=1&sn=1080cb70842f6217a4c5dbfbafec309a&scene=20#wechat_redirect)