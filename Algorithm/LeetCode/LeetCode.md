# LeetCode

## 网站
  - [CodeTop](https://codetop.cc/home)
  - [LeetCode](https://leetcode.cn/problemset/all/)

## 常见算法
###  无重复字符的最长子串
> 给定一个字符串 s ，请你找出其中不含有重复字符的 最长子串 的长度。
```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
       int maxSubLength = 0;
       List<Character> subStr = new ArrayList<>();
       for (int i = 0; i< s.length(); i++) {
           Character curChar = s.charAt(i);
           int existIndex = subStr.indexOf(curChar);
           subStr.add(curChar);
           if (existIndex >= 0) {
               for (int j = existIndex; j >= 0; j-- ){
                   subStr.remove(j);
               }
           } else {
              if (maxSubLength < subStr.size()) {
                maxSubLength = subStr.size();
              }
           }        
       }
       return maxSubLength;  
    }
}
```