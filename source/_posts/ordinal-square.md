---
title: 有序数组的平方
date: 2021-12-01 16:54:07
categories:
- 算法
tags:
- 双指针
---
# 有序数组的平方

## 题目链接
https://leetcode-cn.com/problems/squares-of-a-sorted-array/

## 题目描述
给你一个按 非递减顺序 排序的整数数组 nums，返回 每个数字的平方 组成的新数组，要求也按 非递减顺序 排序。

**示例 1:**

    输入：nums = [-4,-1,0,3,10]
    输出：[0,1,9,16,100]
    解释：平方后，数组变为 [16,1,0,9,100]
    排序后，数组变为 [0,1,9,16,100]
    
 **示例 2:**
 
    输入：nums = [-7,-3,2,3,11]
    输出：[4,9,9,49,121]
 
 **提示：**
 
  * 1 <= nums.length <= 10<sup>4</sup>
  * -10<sup>4</sup> <= nums[i] <= 10<sup>4</sup>
  * nums 已按 非递减顺序 排序

## 解题思路

## 方法一：直接排序

最简单的方法就是将数组 nums 中的数平方后直接排序。

## 代码
```java
class Solution {
    public int[] sortedSquares1(int[] nums) {
        int[] result = new int[nums.length];
        for (int i = 0; i < nums.length; i++) {
            result[i] = nums[i] * nums[i];
        }
        Arrays.sort(result);
        return result;
    }
}
```
## 复杂度分析
 * 时间复杂度：O(nlogn)，其中 n 是数组 nums 的长度。

 * 空间复杂度：O(logn)。除了存储答案的数组以外，我们需要 O(logn) 的栈空间进行排序。
 
## 方法二：双指针
方法一没有利用「数组 nums 已经按照升序排序」这个条件。显然，如果数组 nums 中的所有数都是非负数，那么将每个数平方后，数组仍然保持升序；如果数组 nums 中的所有数都是负数，那么将每个数平方后，数组会保持降序。

这样一来，如果我们能够找到数组 nums 中负数与非负数的分界线，那么就可以用类似「归并排序」的方法了。具体地，我们设 neg 为数组nums 中负数与非负数的分界线，也就是说，nums[0] 到 nums[neg] 均为负数，而 nums[neg+1] 到 nums[n−1] 均为非负数。当我们将数组 nums 中的数平方后，那么 nums[0] 到 nums[neg] 单调递减，nums[neg+1] 到 nums[n−1] 单调递增。

由于我们得到了两个已经有序的子数组，因此就可以使用归并的方法进行排序了。具体地，使用两个指针分别指向位置 neg 和 neg+1，每次比较两个指针对应的数，选择较小的那个放入答案并移动指针。当某一指针移至边界时，将另一指针还未遍历到的数依次放入答案。

## 代码

```java
class Solution {
    public int[] sortedSquares2(int[] nums) {
        int n = nums.length;
        int negative = -1;
        for (int i = 0; i < n; ++i) {
            if (nums[i] < 0) {
                negative = i;
            } else {
                break;
            }
        }

        int[] ans = new int[n];
        int index = 0, i = negative, j = negative + 1;
        while (i >= 0 || j < n) {
            if (i < 0) {
                ans[index] = nums[j] * nums[j];
                ++j;
            } else if (j == n) {
                ans[index] = nums[i] * nums[i];
                --i;
            } else if (nums[i] * nums[i] < nums[j] * nums[j]) {
                ans[index] = nums[i] * nums[i];
                --i;
            } else {
                ans[index] = nums[j] * nums[j];
                ++j;
            }
            ++index;
        }

        return ans;
    }
}

```
## 复杂度分析
   
 * 时间复杂度：O(n)，其中 nn 是数组 nums 的长度。

 * 空间复杂度：O(1)。除了存储答案的数组以外，我们只需要维护常量空间。
 
## 方法三：双指针

同样地，我们可以使用两个指针分别指向位置 0 和 n−1，每次比较两个指针对应的数，选择较大的那个逆序放入答案并移动指针。这种方法无需处理某一指针移动至边界的情况，读者可以仔细思考其精髓所在。
 
## 代码
 
```java
class Solution {
    public int[] sortedSquares3(int[] nums) {
       int n = nums.length;
       int[] result = new int[n];
       for (int i = 0, j = n - 1, pos = n - 1; i <= j; ) {
           if (nums[i] * nums[i] > nums[j] * nums[j]) {
               result[pos] = nums[i] * nums[i];
               ++i;
           } else {
               result[pos] = nums[j] * nums[j];
               --j;
           }
           --pos;
       }
       return result;
   }
}
```

## 复杂度分析

* 时间复杂度：O(n)，其中 n 是数组 nums 的长度。

* 空间复杂度：O(1)。除了存储答案的数组以外，我们只需要维护常量空间。


