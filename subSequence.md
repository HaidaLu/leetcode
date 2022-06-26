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



### 2. 所有递增子序列 Backtracking

- 类似于去重的子集问题, 但是, 子序列 不能改变原有nums的顺序, 因此, 原来的去重问题不能解

- 这里的去重 就是每到新的一层都会建立一个新的used数组(之前一层会清空) ,那就是在for循环前面建立新的used数组即可.

![491. 递增子序列1](https://img-blog.csdnimg.cn/20201124200229824.png)

```java
class Solution {
    List<List<Integer>> res = new LinkedList<>();
    LinkedList<Integer> track = new LinkedList<>();
    public List<List<Integer>> findSubsequences(int[] nums) {
        backtrack(nums, 0);
        return res;
    }
    
    private void backtrack(int[] nums, int start) {
        // end condition
        if (track.size() > 1) {
            res.add(new LinkedList(track));
        }
        int[] used = new int[201]; // 只定义本层 到了新的一层会被清空
        
        for (int i = start; i < nums.length; i++) {
            if (!track.isEmpty() && nums[i] < track.get(track.size() - 1) || (used[nums[i] + 100] == 1)) {
                continue;
            }
            used[nums[i] + 100] = 1;
            track.add(nums[i]);
            
            backtrack(nums, i+1);
            
            track.removeLast();
        }
        //     4767
    }
}
```

