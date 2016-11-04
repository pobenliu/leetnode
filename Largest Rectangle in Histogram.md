# Largest Rectangle in Histogram

 Largest Rectangle in Histogram ( [leetcode]() [lintcode](http://www.lintcode.com/en/problem/largest-rectangle-in-histogram/) )

```markdown
Description
Given n non-negative integers representing the histogram's bar height 
where the width of each bar is 1, 
find the area of largest rectangle in the histogram.
```

![](http://ww3.sinaimg.cn/mw690/600e6311jw1f7qxhtrqnfj205805oglh.jpg)

```
Above is a histogram where width of each bar is 1, given height = [2,1,5,6,2,3].
```

![](http://ww2.sinaimg.cn/mw690/600e6311jw1f7qxhuelh6j205805oaab.jpg)

```
The largest rectangle is shown in the shaded area, which has area = 10 unit.

Example
Given height = [2,1,5,6,2,3],
return 10.
```

### 解题思路

#### 一、遍历法

遍历所有可能的起点、终点，及对应宽度内高度的最小值，得到所有可能取得的矩形面积值，取最大值。

##### 算法复杂度

- 时间复杂度：由于双重循环，所以复杂度为 `O(n^2)` 。

Java 实现

```java
public class Solution {
    /**
     * @param height: A list of integer
     * @return: The area of largest rectangle in the histogram
     */
    public int largestRectangleArea(int[] height) {
        // write your code here
        if (height == null || height.length == 0) {
            return 0;
        }
        int n = height.length;
        int max = 0;
        for (int i = 0; i < n; i++) {
            int H = height[i];
            int W = 0;
            for (int j = i; j < n; j++) {
                H = Math.min(H, height[j]);
                W = i - j + 1;
                max = Math.max(max, H * W);
            }
        }
        
        return max;
    }
}
```



#### 二、递增栈

##### 思路：

对于每一个高度，只要知道它左边第一个比它小的数，它右边第一个比它小的数，那么就能求出这个高度对应的最大矩形的面积。这种情况需要使用辅助栈。

##### 实现方法：

使用辅助栈，存储当前数组元素序号，每次比较栈顶值与当前元素的大小，如果当前值大于栈顶值，入栈。如果栈顶值小于当前值，弹出栈顶值，并计算栈顶值高度对应的面积。

由于栈顶值一定比栈中相邻值大，所以栈顶值高度为 `height(stack.pop())` ，而宽度为 `i - stack.peek() - 1` ，之所以要 `-1` 是因为栈顶值已弹出。当所有元素都已完成入栈，并且全部完成出栈，那么最后一个出栈的元素值对应的长度是直方图长度 `height.length` 。因为这是所有高度中的最小值，所以对应矩形宽度为直方图的宽度。

##### 算法复杂度

- 时间复杂度：一次遍历，为 `O(n)`。

Java 实现

```java
public class Solution {
    /**
     * @param height: A list of integer
     * @return: The area of largest rectangle in the histogram
     */
    public int largestRectangleArea(int[] height) {
        // write your code here
        if (height == null || height.length == 0) {
            return 0;
        }
        
        Stack<Integer> stack = new Stack<Integer>();
        int max = 0;
        // when i == height.length. all valid i has been pushed into the stack
        // deal with the values in stack
        for (int i = 0; i <= height.length; i++) {
            int curt = (i == height.length) ? -1 : height[i];
            while (!stack.isEmpty() && curt <= height[stack.peek()]) {
                int H = height[stack.pop()];
                int W = stack.isEmpty() ? i : i - stack.peek() - 1;
                max = Math.max(max, H * W);
            }
            stack.push(i);
        }
        
        return max;
    }
}
```



### 参考

1. [Largest Rectangle in Histogram 解题报告 | 水中的鱼](http://fisherlei.blogspot.jp/2012/12/leetcode-largest-rectangle-in-histogram.html)
2. [LeetCode: Largest Rectangle in Histogram 解题报告 | Yu's graden](http://www.cnblogs.com/yuzhangcmu/p/4191981.html)
3. [Largest Rectangle in Histogram | 九章算法](http://www.jiuzhang.com/solutions/largest-rectangle-in-histogram/)