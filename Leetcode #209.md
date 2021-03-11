#### 题目：Leetcode #209

> 滑动窗口

给定一个含有 n 个正整数的数组和一个正整数 target 。

找出该数组中满足其和 ≥ target 的长度最小的 连续子数组 [numsl, numsl+1, ..., numsr-1, numsr] ，并返回其长度。如果不存在符合条件的子数组，返回 0 。

```java
    public int minSubArrayLen(int target, int[] nums) {
        
    }
```



#### 解题

这类线性结构，链表和数组，求连续长度的最值，都可以用滑动窗口的方法，通过两个指针，一前一后移动，控制窗口的大小，计算窗口内的值与目标要求的情况，来判断是移动后一个指针，还是移动前一个指针。

```java
public int minSubArrayLen(int target, int[] nums) {
        if (0 == nums.length) {
            return 0;
        }
        int res = Integer.MAX_VALUE;
        int left = 0;
        int right = left;
        int sum = nums[left];
        // 左右窗口，当右窗口到达边界，且sum值小于target，遍历结束
        // 当sum >= target，则计算右窗口到左窗口长度，并与res比较，小于res则更新res值，左窗口向前移动一格
        // 当sum < target, 则右窗口向前移动一格
        while (right < nums.length-1 || sum >= target) {
            if (sum >= target) {
                int length = right - left + 1;
                if (left == right) {
                    return length;
                }
                if (length < res) {
                    res = length;
                }
                sum -= nums[left];
                left++;
            } else {
                if (right < nums.length-1) {
                    right++;
                    sum += nums[right];
                }
            }
        }

        return Integer.MAX_VALUE == res ? 0 : res;
    }
```

下面这种也是滑动窗口，如果sum大于目标值，一直挪动左边，减少窗口大小和sum，并更新长度，否则移动右窗口，增加sum值。一旦sum大于目标值，则重复上面的操作。

```java
class Solution {
    public int minSubArrayLen(int s, int[] nums) {
        int n = nums.length;
        if (n == 0) {
            return 0;
        }
        int ans = Integer.MAX_VALUE;
        int start = 0, end = 0;
        int sum = 0;
        while (end < n) {
            sum += nums[end];
            while (sum >= s) {
                ans = Math.min(ans, end - start + 1);
                sum -= nums[start];
                start++;
            }
            end++;
        }
        return ans == Integer.MAX_VALUE ? 0 : ans;
    }
}

```



> 前缀和加二分查找 nlogn
