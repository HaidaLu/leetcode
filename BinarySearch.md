### 题目列表

[704. Binary Search](https://leetcode.com/problems/binary-search/)

[34. Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

### Summary

- right = nums.length - 1 -> while (left <= right)

  right = nums.length -> while (left < right)

  right = nums.length ->搜索区间为[left, right) , 右侧不取 -> `while(left < right)` -> 终止的条件是 `left == right`，此时搜索区间 `[left, left)` 为空，所以可以正确终止 -> left = mid + 1/ right = mid -> 比较过mid之后, 要取[mid+1, right) 或者[left, mid) 

  

### 1. Base code: 

- Ascending order
- all the integers in nums are unique

```java
class Solution {
    public int search(int[] nums, int target) {
        int left = 0;
        int right = nums.length - 1;
        while(left <= right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] == target) return mid;
            else if (nums[mid] < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return -1;
    }
}
```

1. Right: nums.length - 1 
2. left <= right 
3. mid = left + (right - left) / 2
4. left = mid + 1 / right = mid - 1



### 2. Find LeftOver Target

Code:

```java
public int findLeftOverBS(int[] nums, int target) {
  int left = 0, right = nums.length;
  while(left < right){
    int mid = left + (right - left) / 2;
    if (nums[mid] >= target) {
      right = mid;
    } else {
      left = mid + 1;
    }
  }
  return left >= nums.length  || nums[left] != target ? -1 : left; 
}
```

1. right = mid/ left = mid + 1

   「搜索区间」是 `[left, right)` 左闭右开，所以当 `nums[mid]` 被检测之后，下一步应该去 `mid` 的左侧或者右侧区间搜索，即 `[left, mid)` 或 `[mid + 1, right)`。 

2. left < right;

3. return left (not -1) (it depends)

4. 此时, 这个左侧边界可以理解为: 小于target的有几个

5. 可能的极限情况是left >= nums.length 因为 左边一直加大

> right = nums.length -> while (left < right) -> left = mid + 1/ right = mid
>
> right = nums.length ->搜索区间为[left, right) , 右侧不取 -> `while(left < right)` -> 终止的条件是 `left == right`，此时搜索区间 `[left, left)` 为空，所以可以正确终止 -> left = mid + 1/ right = mid -> 比较过mid之后, 要取[mid+1, right) 或者[left, mid) 
>
> 

### 3. Find RightOver Target 

```java
public int searchRightOver(int[] nums, int target) {
        int left = 0, right = nums.length; // nums.length or nums.length - 1?
        while (left < right) {  // < or <= ?
            int mid = left + (right - left) / 2;
            if (nums[mid] <= target) {
                left = mid + 1;                // = mid or = mid 
            } else if (nums[mid] > target) {
                right = mid;
            } 
        }
        
        return left < 1 || nums[left - 1] != target? -1 : left - 1;
    }
```

- nums[mid] <= target -> left = mid + 1
- return left - 1
  - 对 `left` 的更新必须是 `left = mid + 1`，就是说 while 循环结束时，`nums[left]` 一定不等于 `target` 了，而 `nums[left-1]` 可能是 `target`。

