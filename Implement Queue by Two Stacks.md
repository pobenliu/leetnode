#  Implement Queue by Two Stacks

 Implement Queue by Two Stacks ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/implement-queue-by-two-stacks/) )

```
Description
As the title described, you should only use two stacks to implement a queue's actions.
The queue should support push(element), pop() and top() 
where pop is pop the first(a.k.a front) element in the queue.
Both pop and top methods should return the value of first element.
  
Example
push(1)
pop()     // return 1
push(2)
push(3)
top()     // return 2
pop()     // return 2

Challenge
implement it by two stacks, do not use any other data structure and push, 
pop and top should be O(1) by AVERAGE.
```



### 解题思路

经典题目，使用两个栈实现队列。进入队列时将元素压入 `stack1` ，出队列时从 `stack2` 取出，其中 `stack2` 中的元素是将 `stack1` 依次放入，所以取出时的顺序符合先进先出。

Java 实现

```java
public class Queue {
    private Stack<Integer> stack1;
    private Stack<Integer> stack2;

    public Queue() {
       // do initialization if necessary
       stack1 = new Stack<Integer>();
       stack2 = new Stack<Integer>();
    }
    
    public void push(int element) {
        // write your code here
        stack1.push(element);
    }

    public int pop() {
        // write your code here
        if (stack2.isEmpty()) {
            stack2Tostack1();
        }
        return stack2.pop();
    }

    public int top() {
        // write your code here
        if (stack2.isEmpty()) {
            stack2Tostack1();
        }    
        return stack2.peek();
    }
    
    public void stack2Tostack1 () {
        while (!stack1.isEmpty()) {
            stack2.push(stack1.pop());
        }
    }
}
```



### 参考







