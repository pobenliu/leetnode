#  Interleaving Positive and Negative Numbers

 Interleaving Positive and Negative Numbers  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/interleaving-positive-and-negative-numbers/) )

```
Description
Given an array with positive and negative integers. Re-range it to interleaving with positive and negative integers.

Notice
You are not necessary to keep the original order of positive integers or negative integers.

Example
Given [-1, -2, -3, 4, 5, 6], after re-range, it will be [-1, 5, -2, 4, -3, 6] or any other reasonable answer.

Challenge 
Do it in-place and without extra memory.
```

### 解题思路

#### 一、使用额外空间

- 将正数、负数依次放入两个新建的数组
- 如果正数多，则按照先放正数，再放负数的顺序，依次将两个数组中元素交织放在一起。如果负数多，则放置顺序相反。

Java 实现

```java
class Solution {
    /**
     * @param A: An integer array.
     * @return: void
     */
    public void rerange(int[] A) {
        if (A == null || A.length == 0) {
            return;
        }
        
        int[] Ap = new int[A.length];
        int[] Am = new int[A.length];
        int[] tmp = new int[A.length];
        int totp = 0;
        int totm = 0;
        
        for (int i = 0; i < A.length; i++) {
            if (A[i] > 0) {
                Ap[totp] = A[i];
                totp++;
            } else {
                Am[totm] = A[i];
                totm++;
            }
        }
        
        if (totp > totm) {
            tmp = subfun(Ap, Am, A.length);
        } else {
            tmp = subfun(Am, Ap, A.length);
        }
        
        for (int i = 0; i < tmp.length; i++) {
            A[i] = tmp[i];
        }
   }
   
   private int[] subfun (int[] A, int[] B, int len) {
       int[] ans = new int[len];
       for (int i = 0; i * 2 + 1 < len; i++) {
           ans[i * 2] = A[i];
           ans[i * 2 + 1] = B[i];
       }
       
       if (len % 2 == 1) {
           ans[len - 1] = A[len / 2];
       } 
       
       return ans;
   }
   
}
```



#### 二、不适用额外空间

- 先判断正数多，还是负数多，以决定两者在数组中排列的相对顺序，数量多的从数组第一个开始放，这样就能够包裹住数量少的，数组尾部全是数量多的数。以 `[1, 2, 3, -4, -5]` 为例，正数多，则调整后数组应该为 `[1, -4, 3, -5, 2]` ，数组首元素是正数，第二个是负数。
- 使用双指针，依次判断所在位置是否是应该放置的数，如奇数位置放正数、偶数位置放负数，找到不符合要求的，进行交换。这里需要注意索引遍历的间隔是 2 。

Java 实现

```java
class Solution {
    /**
     * @param A: An integer array.
     * @return: void
     */
    public void rerange(int[] A) {
        if (A == null || A.length == 0) {
            return;
        }
        
        int posNum = 0;
        int negNum = 0;
        for (int i = 0; i < A.length; i++) {
            if (A[i] < 0) {
                negNum++;
            } else {
                posNum++;
            }
        }
        
        int posInd = 1;
        int negInd = 0;
        if (posNum > negNum) {
            posInd = 0;
            negInd = 1;
        }
        
        while (posInd < A.length && negInd < A.length) {
            while (posInd < A.length && A[posInd] > 0) {
                posInd += 2;
            }
            while (negInd < A.length && A[negInd] < 0) {
                negInd += 2;
            }
            
            if (posInd < A.length && negInd < A.length) {
                exch(A, posInd, negInd);
            }
        }
   }
   
   private void exch(int[] A, int i, int j) {
       int temp = A[i];
       A[i] = A[j];
       A[j] = temp;
   }
   
}
```



### 参考

1. [Interleaving Positive and Negative Numbers | 九章算法](Interleaving Positive and Negative Numbers)

2. [Lintcode: Interleaving Positive and Negative Numbers | neverlandly](http://www.cnblogs.com/EdwardLiu/p/4314781.html)

   ​

