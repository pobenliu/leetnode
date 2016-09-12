#  Min Stack

 Min Stack  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/min-stack/) )

```
Description
Implement a stack with min() function, which will return the smallest number in the stack.
It should support push, pop and min operation all in O(1) cost.

Notice
min operation will never be called if there is no number in the stack.

Example
push(1)
pop()   // return 1
push(2)
push(3)
min()   // return 2
push(1)
min()   // return 1
```



### 解题思路

题目要求是 `O(1)` 时间的 `min` 操作，对空间复杂度则没有要求，考虑使用两个栈，其中一个栈用作正常用处，另一个栈则同步存储对应的最小值。

Java 实现

```java
public class MinStack {

    private Stack<Integer> nums;
    private Stack<Integer> mins; 
    
    public MinStack() {
        // do initialize if necessary
        nums = new Stack<Integer>();
        mins = new Stack<Integer>();
    }

    public void push(int number) {
        // write your code here
        nums.push(number);
        if (mins.isEmpty()) {
            mins.push(number);
        } else {
            mins.push(Math.min(number, mins.peek()));
        }
    }

    public int pop() {
        // write your code here
        mins.pop();
        return nums.pop();
    }

    public int min() {
        // write your code here
        return mins.peek();
    }     
}
```



优化解法：

只有在进栈元素小于等于当前最小值时，才将其加入最小值栈；进栈元素大于当前最大值时，最小值栈无需操作。出栈时需要做同样的判断。

其中，进栈元素等于最小值时，为避免出栈时出现问题，故需要对最小值栈进行操作。

Java 实现

```java
public class MinStack {

    private Stack<Integer> nums;
    private Stack<Integer> mins; 
    
    public MinStack() {
        // do initialize if necessary
        nums = new Stack<Integer>();
        mins = new Stack<Integer>();
    }

    public void push(int number) {
        // write your code here
        nums.push(number);
        if (mins.isEmpty()) {
            mins.push(number);
        } else if (number <= mins.peek()) {
            mins.push(number);
        } 
    }

    public int pop() {
        // write your code here
        // attention for diffrence between .equals() and ==
        // also attention for .peek() and pop()
        if (nums.peek().equals(mins.peek())) {
            mins.pop();
        } 
        return nums.pop();    
    }

    public int min() {
        // write your code here
        return mins.peek();
    }     
}

```



### 参考

1. [Min Stack | 九章算法](http://www.jiuzhang.com/solutions/min-stack/)

