### 题目列表

[704. Binary Search](https://leetcode.com/problems/binary-search/)

[34. Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

[35. Search Insert Position(Find LeftOver Target)](https://leetcode.com/problems/search-insert-position/)

[875. Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/)

[1011. Capacity To Ship Packages Within D Days](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/)

## 基础二分搜索: 搜索一个元素

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



## 进阶二分搜索: 搜索边界

算法题一般是求最值, 比如求吃香蕉的**最小速度**, 求轮船的**最低运载能力**, 求最值的过程, 必然是搜索一个边界的过程. 比如, 搜索最左边界, 可以这么想象:

![img](https://labuladong.github.io/algo/images/%e4%ba%8c%e5%88%86%e8%bf%90%e7%94%a8/1.jpeg)

### 二分搜索问题的泛化

什么问题可以运用二分搜索算法技巧? -> 数学角度思考

从题目中抽象出一个自变量`x`, 一个关于`x`的函数`f(x)`, 以及一个目标值`target`, 并满足以下条件:

1. f(x)必须是在x上的单调函数(单调增单调减都可以) **monotonically increasing/decreasing**
2. 题目是让你计算满足约束条件f(x) == target时的x的值

```java
// 函数 f 是关于自变量 x 的单调递增函数
int f(int x, int[] nums) {
    return nums[x];
}

int left_bound(int[] nums, int target) {
    if (nums.length == 0) return -1;
    int left = 0, right = nums.length;
    
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (f(mid, nums) == target) {
            // 当找到 target 时，收缩右侧边界
            right = mid;
        } else if (f(mid, nums) < target) {
            left = mid + 1;
        } else if (f(mid, nums) > target) {
            right = mid;
        }
    }
    return left;
}
```

### 运用二分搜索的套路框架

```java
// 函数 f 是关于自变量 x 的单调函数
int f(int x) {
    // ...
}

// 主函数，在 f(x) == target 的约束下求 x 的最值
int solution(int[] nums, int target) {
    if (nums.length == 0) return -1;
    // 问自己：自变量 x 的最小值是多少？
    int left = ...;
    // 问自己：自变量 x 的最大值是多少？
    int right = ... + 1;
    
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (f(mid) == target) {
            // 问自己：题目是求左边界还是右边界？
            // ...
        } else if (f(mid) < target) {
            // 问自己：怎么让 f(x) 大一点？
            // ...
        } else if (f(mid) > target) {
            // 问自己：怎么让 f(x) 小一点？
            // ...
        }
    }
    return left;
}
```



#### 875. 求满足h的吃香蕉的最小速度 Koko Eating Bananas

```java
class Solution {
    public int minEatingSpeed(int[] piles, int h) {
        int left = 1;
        int right = 1000000000 + 1;
        
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (f(piles, mid) <= h) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
        
    }
    
    int f(int[] piles, int x) {
        int hours = 0;
        for (int i = 0; i < piles.length; i++) {
            hours += piles[i] / x;
            if (piles[i] % x != 0) {
                hours++;
            }
        }
        return hours;
    }
}

// what is x?
// what is f(x)?
```



#### 1011. 在 D 天内送达包裹的能力 Capacity To Ship Packages Within D Days -> 1. f(x)的代码很不好写 2. 注意right取值

```java
class Solution {
    public int shipWithinDays(int[] weights, int days) {
        int left = 0;
        int right = 1;
        for (int w : weights) {
            left = Math.max(left, w);
            right += w;
        }
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (f(weights, mid) <= days) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
    private int f(int[] weights, int weight) {
        int days = 0;
        //int w = weight;
        for (int i = 0; i < weights.length; ) {
            int w = weight;
            while (i < weights.length) {
                if (w < weights[i]) break;
                else w -= weights[i];
                i++;
            }
            days++;
        }
        return days;
        
    }
}
```





