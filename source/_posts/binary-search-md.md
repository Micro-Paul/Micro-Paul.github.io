---
title: 算法：二分查找
date: 2021-11-18 11:24:07
categories:
- 算法
tags:
- 二分查找
---
# 二分查找

## 题目描述

给定一个 n 个元素有序的（升序）整型数组 nums 和一个目标值 target  ，写一个函数搜索 nums 中的 target，如果目标值存在返回下标，否则返回 -1。

**示例 1:**
    
    输入: nums = [-1,0,3,5,9,12], target = 9
    输出: 4
    解释: 9 出现在 nums 中并且下标为 4
    
 **示例 2:**
 
    输入: nums = [-1,0,3,5,9,12], target = 2
    输出: -1
    解释: 2 不存在 nums 中因此返回 -1
 
 
 **提示:**
 
 1. 你可以假设 nums 中的所有元素是不重复的。
 2. n 将在 [1, 10000]之间。
 3. nums 的每个元素都将在 [-9999, 9999]之间。
 
## 解题思路
 
 * 标签：二分查找
 * 过程
     * 设定左右指针
     * 找出中间位置，并判断该位置值是否等于 target
     * nums[mid] == target 则返回该位置下标
     * nums[mid] > target 则右侧指针移到中间
     * nums[mid] < target 则左侧指针移到中间
 * 时间复杂度：O(logN) 其中 N 是数组的长度。
 * 空间复杂度：O(1)。
  
## 代码
```java
public class Solution {

    public static void main(String[] args) {
        Solution test = new Solution();
        int[] nums = {-1, 0, 3, 5, 9, 12};
        System.out.println(test.search(nums, 9));
    }

    public int search(int[] nums, int target) {
        int left = 0;
        int right = nums.length - 1;
        while (left <= right) {
            int mid = (left + right) / 2;
            if (nums[mid] == target) {
                return mid;
            } else if (nums[mid] < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return -1;
    }
}
```

