# Reverse Words in a String

 Reverse Words in a String  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/reverse-words-in-a-string/#) )

```
Description
Given an input string, reverse the string word by word.
For example,
Given s = "the sky is blue",
return "blue is sky the".

Clarification
- What constitutes a word?
  A sequence of non-space characters constitutes a word.
- Could the input string contain leading or trailing spaces?
  Yes. However, your reversed string should not contain leading or trailing spaces.
- How about multiple spaces between two words?
  Reduce them to a single space in the reversed string.
```

### 解题思路

本题中只有空格字符、非空格字符之分，所以在处理单词时，如何处理空格是比较关键的。

#### 一、使用 `split` 函数

Java 中的 `split` 函数可以根据一定的规则将字符串分割为一个字符串数组，

> 函数声明： `public String[] split(String regex)`
>
> 以字符串 `“boo:and:foo”` 为例，不同的参数可以得到不同的结果。
>
> ​	参数		结果
>
> ​	  `:`			`{ "boo", "and", "foo" }`
>
> ​	  `o`			`{ "b", "", ":and:f" }`

在实现过程还用到了 `StringBuilder` 类型，简要介绍一下它与 `String` ， `StringBuffer` 的异同：

|       | String  | StringBuffer     | StringBuilder    |
| ----- | ------- | ---------------- | ---------------- |
| 存储区域  | 常量字符串池  | 堆                | 堆                |
| 可修改性  | 不可修改    | 可修改              | 可修改              |
| 线程安全性 | 线程安全    | 线程安全             | 非线程              |
| 性能    | 快       | 很慢               | 快                |
| 适用情况  | 字符串无需修改 | 字符串需修改，且会被多个线程访问 | 字符串需修改，只会被一个线程访问 |



Java 实现

```java
public class Solution {
    /**
     * @param s : A string
     * @return : A string
     */
    public String reverseWords(String s) {
        if (s == null || s.length() == 0) {
            return "";
        }
        
        String[] array = s.split(" ");
        StringBuilder sb = new StringBuilder();
        
        for (int i = array.length - 1; i >= 0; i--) {
            if (!array[i].equals("")) {
                sb.append(array[i]).append(" ");
            }
        }
        
        // remove the last " "
        return sb.length() == 0 ? "" : sb.substring(0, sb.length() - 1);
    }
}

```



#### 二、逐个字符处理

题目要求所有单词要逆序排列，从前往后取单词比较麻烦，考虑从后往前处理。在处理空格时需要小心，如何判断处理“字符串最后的空格”和“单词之间的空格”。

- 当遇到“字符串最后的空格”时，此时 `subString` 为空，以此为判断条件跳过空格。
- 当取出单词，遇到第一个“单词之间的空格”时，此时 `subString` 非空，将其添加入 `result` 中。如果此时 `result` 非空要加空格。之后将 `subString` 置为空。

感觉上述实现稍微有点绕 = =!!!

Java 实现

```java
public class Solution {
    /**
     * @param s : A string
     * @return : A string
     */
    public String reverseWords(String s) {
        if (s == null || s.length() == 0) {
            return s;
        }
        
        StringBuilder subString = new StringBuilder();
        StringBuilder result = new StringBuilder();
        
        for (int i = s.length() - 1; i >= 0; i--) {
            if (s.charAt(i) != ' ') {
                subString.append(s.charAt(i));
            } else if (subString.length() != 0) {
                if (result.length() != 0) {
                    result.append(" ");
                }
                result.append(subString.reverse());
                subString = new StringBuilder();
            }
        }
        
        if (subString.length() != 0) {
            if (result.length() != 0) {
                result.append(" ");
            }
            result.append(subString.reverse());
        }
        return result.toString();
    }
}

```



### 参考

1. [Reverse Words in a String | 九章算法](http://www.jiuzhang.com/solutions/reverse-words-in-a-string/)
2. [Difference Between String , StringBuilder And StringBuffer Classes With Example : Java | JAVA HUNGRY](http://javahungry.blogspot.com/2013/06/difference-between-string-stringbuilder.html)
3. [String, StringBuffer, and StringBuilder | stackoverflow](http://stackoverflow.com/questions/2971315/string-stringbuffer-and-stringbuilder)
4. [[LeetCode] Reverse Words in a String | 努橙刷题编](http://codechen.blogspot.jp/2015/06/leetcodereverse-words-in-string.html)