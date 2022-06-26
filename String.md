

### 判断字符串是否回文

```java
boolean isPalindrome(String s, int left, int right) {
  while (left <= right) {
    if (s.charAt(left) != s.charAt(right)) return false;
    left++;
    right--;
  }
  return true;
}
```

