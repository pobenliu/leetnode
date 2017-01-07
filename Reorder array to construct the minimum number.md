#  Reorder array to construct the minimum number

 Reorder array to construct the minimum number  ( [lintcode](http://www.lintcode.com/en/problem/reorder-array-to-construct-the-minimum-number/) )

```
Description
Construct minimum number by reordering a given non-negative integer array. 
Arrange them such that they form the minimum number.

Notice
The result may be very large, so you need to return a string instead of an integer.

Example
Given [3, 32, 321], there are 6 possible numbers can be constructed by reordering the array:
3+32+321=332321
3+321+32=332132
32+3+321=323321
32+321+3=323213
321+3+32=321332
321+32+3=321323
So after reordering, the minimum number is 321323, and return it.
```

### 解题思路

使用排序算法，重新定义字符串排序规则。

Java 实现

```java
public class Solution {
    /**
     * @param nums n non-negative integer array
     * @return a string
     */
    public String minNumber(int[] nums) {
        // Write your code here
        if (nums == null || nums.length == 0) {
            return "";
        }
        
        String[] s = new String[nums.length];
        for (int i = 0; i < nums.length; i++) {
            s[i] = String.valueOf(nums[i]);
        }
        
        Arrays.sort(s, new Comparator<String>() {
            public int compare(String s1, String s2) {
                String s12 = s1.concat(s2);
                String s21 = s2.concat(s1);
                return s12.compareTo(s21);
            }
        });
        
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < s.length; i++) {
            sb.append(s[i]);
        }
        for (int i = 0; i < sb.length(); i++) {
            if (sb.charAt(i) != '0') {
                return sb.substring(i);
            }
        }
        
        return "0";
    }
}
```

