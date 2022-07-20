

[20. Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)

[921. Minimum Add to Make Parentheses Valid](https://leetcode.com/problems/minimum-add-to-make-parentheses-valid/)

[1541. Minimum Insertions to Balance a Parentheses String](https://leetcode.com/problems/minimum-insertions-to-balance-a-parentheses-string/)

[32. Longest Valid Parentheses](https://leetcode.com/problems/longest-valid-parentheses/?show=1)

[22. Generate Parentheses](https://leetcode.com/problems/generate-parentheses/)

[496. Next Greater Element I](https://leetcode.com/problems/next-greater-element-i/)

[503. Next Greater Element II](https://leetcode.com/problems/next-greater-element-ii/)

[739. Daily Temperatures](https://leetcode.com/problems/daily-temperatures/)

[239. Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)

## 1. 括号问题

`res` 记录的遍历到i处左边的插入次数(，`need` 记录了右括号的需求

[20. Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)

[921. Minimum Add to Make Parentheses Valid](https://leetcode.com/problems/minimum-add-to-make-parentheses-valid/)

[1541. Minimum Insertions to Balance a Parentheses String](https://leetcode.com/problems/minimum-insertions-to-balance-a-parentheses-string/)

[32. Longest Valid Parentheses](https://leetcode.com/problems/longest-valid-parentheses/?show=1)

### 32. Longest Valid Parentheses

```java
class Solution {
    public int longestValidParentheses(String s) {
        int left = 0, right = 0, maxlength = 0;
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == '(') {
                left++;
            } else {
                right++;
            }
            if (left == right) {
                maxlength = Math.max(maxlength, 2 * right);
            } else if (right > left) {
                left = right = 0;
            }
        }
        left = right = 0;
        for (int i = s.length() - 1; i>= 0; i--) {
            if (s.charAt(i) == '(') {
                left++;
            } else {
                right++;
            }
            if (left == right) {
                maxlength = Math.max(maxlength, 2 * right);
            } else if (left > right) {
                left = right = 0;
            }
        }
        return maxlength;
        
    }
}
```



## 2. 单调栈

[496. Next Greater Element I](https://leetcode.com/problems/next-greater-element-i/)

[503. Next Greater Element II](https://leetcode.com/problems/next-greater-element-ii/)

[739. Daily Temperatures](https://leetcode.com/problems/daily-temperatures/)

## 3. 单调队列

[239. Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)

