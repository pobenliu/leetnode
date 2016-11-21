# Letter Combinations of a Phone Number

 Letter Combinations of a Phone Number  ( [leetcode]()  [lintcode](http://www.lintcode.com/en/problem/letter-combinations-of-a-phone-number/) )

```
Description
Given a digit string excluded 01, 
return all possible letter combinations that the number could represent.
A mapping of digit to letters (just like on the telephone buttons) is given below.
```

![](http://ww1.sinaimg.cn/mw690/600e6311jw1f8bedncvvsj205k04ijri.jpg)

```
Notice
Although the above answer is in lexicographical order, your answer could be in any order you want.

Example
Given "23"
Return ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"]
```

### 解题思路

和 Subsets、Permutations 的实现类似，通过遍历找到所有可能的组合。

需要注意的地方有：

1. 建立一个数字到字母的对照表。
2. 注意 String 、 StringBuilder 相关的函数及操作。
   - String ： 
     - 第 `i` 个字符 `s.charAt(i)` ；
     - 字符串相等 `s.equals(t)` ；
     - 空字符串 `s.isEmpty()` ；
     - 字符串长度 `s.length()` ；
     - 子字符串（索引范围 `start ~ end-1` ） `s.substring(start, end)` ；
     - 转换为字符数组 `s.toCharArray()` 。
   - StringBuilder ：
     - 添加字符或字符串 `sb.append(s)` ；
     - 第 `i` 个字符 `sb.charAt(i)` ；
     - 删除第 `i` 个字符 `sb.deleteCharAt(i)` ；
     - 字符串大小 `sb.length()` ；
     - 转化为 `string` 类型 `sb.toString()` 。
3. `digits` 字符串中的数字需要递增遍历，每个数字对应的字母需要从头遍历。

Java 实现

```java
public class Solution {
    /**
     * @param digits A digital string
     * @return all posible letter combinations
     */
    public ArrayList<String> letterCombinations(String digits) {
        ArrayList<String> result = new ArrayList<>();
        if (digits == null || digits.length() == 0) {
            return result;
        }
        
        String[] dict = {"abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
        
        StringBuilder path = new StringBuilder();
        
        search(digits, path, 0, dict, result);
        return result;
    }
    
    private void search (String digits,
                        StringBuilder path,
                        int pos,
                        String[] dict,
                        ArrayList<String> result) {
        
        if (path.length() == digits.length()) {
            result.add(path.toString());
            return;
        }          
        
        String letter = dict[digits.charAt(pos) - '0' - 2];
        for (int j = 0; j < letter.length(); j++) {
            path.append(letter.charAt(j));
            search(digits, path, pos + 1, dict, result);
            path.deleteCharAt(path.length() - 1);
        }

    }
}
```



### 参考

1. [[LeetCode] Letter Combinations of a Phone Number (Java) | Life In Code](http://www.lifeincode.net/programming/leetcode-letter-combinations-of-a-phone-number-java/)