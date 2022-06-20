# 子序列问题

**子序列**: 不用一定要连续一样, 但是字母顺序要一样 ( `"ace"` is a subsequence of `"abcde"` while `"aec"` is not).)

### 1. 基本子序列 判断: 双指针

```java
class Solution {
    private int left = 0;
    public boolean isSubsequence(String s, String t) {
        int i = 0, j = 0;
        while(i < s.length() && j < t.length()) {
            if (s.charAt(i) == t.charAt(j)) {
                i++;
            }
            j++;
        }
        return i == s.length();
    }
}

```



