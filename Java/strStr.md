# strStr

strStr (leetcode [lintcode](http://www.lintcode.com/en/problem/strstr/))

```
For a given source string and a target string, you should output the first index(from 0) of target string in source string.

If target does not exist in source, just return -1.  

Clarification  
- Do I need to implement KMP Algorithm in a real interview?
- Not necessary. When you meet this problem in a real interview, the interviewer may just want to test your basic implementation ability. But make sure your confirm with the interviewer first.

Example
If source = "source" and target = "target", return -1.
If source = "abcdabcdefg" and target = "bcd", return 1.

Challenge 
O(n2) is acceptable. Can you implement an O(n) algorithm? (hint: KMP)
```

#### 解题思路：

**思路一：穷举法**

- 从被搜索字符串的起始位置开始，逐一与目标字符串进行比较，直至找到，或者搜索结束。

算法性能：

- 最坏情况需要~MN次比较，如`source = "aaaa…aaaaab", target = "aaab"`。

```java
class Solution {
    /**
     * Returns a index to the first occurrence of target in source,
     * or -1  if target is not part of source.
     * @param source string to be scanned.
     * @param target string containing the sequence of characters to match.
     */
    public int strStr(String source, String target) {
        //write your code here
        if (source == null || target == null) {
            return -1;
        }        
        int M = target.length();
        int N = source.length();
        //此处应注意i的取值范围
        for (int i = 0; i <= N - M; i++) {
            int j;  //注意此处声明j的位置，要考虑后面j==M判断
            for (j = 0; j < M; j++) {
                //charAt()是一个方法，所以要使用"()”，而不应使用"[]"
                if (source.charAt(i + j) != target.charAt(j)) {
                    break;   //如果遇到不同的元素，从循环中跳出
                }
            }
            if (j == M) {
                return i;   //返回发现字符串的起始位置
            }
        }
        return -1;   //未找到目标字符串
    }
}
```



**思路二：KMP(Knuth-Morris-Pratt)**





#### 类似题目：



#### 延伸阅读：

1. 字符串查找可以用在诸多领域，如：
   - 垃圾邮件检测：识别特定字符串
   - 电子监视：识别网络流量中的特定字符串
   - 屏幕抓取：从网页中提取相关内容
   - Java函数库：indexOf()方法返回指定字符串出现的第一次位置

