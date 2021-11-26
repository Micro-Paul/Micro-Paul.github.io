---
title: 算法：二分查找
date: 2021-11-26 10:55:12
categories:
- 算法
tags:
- 二分查找
---

# 搜索插入位置

## 题目链接
https://leetcode-cn.com/problems/search-insert-position/

## 题目描述

给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。

请必须使用时间复杂度为 O(log n) 的算法。

 **示例 1:**

    输入: nums = [1,3,5,6], target = 5
    输出: 2
    
 **示例 2:**
 
    输入: nums = [1,3,5,6], target = 2
    输出: 1
 
 **示例 3:**
  
    输入: nums = [1,3,5,6], target = 7
    输出: 4
  
 **示例 4:**
   
    输入: nums = [1,3,5,6], target = 0
    输出: 0
      
 **示例 5:**
         
    输入: nums = [1], target = 0
    输出: 0
   
 **提示：**
 
  * 1 <= nums.length <= 104
  * -10<sup>4</sup> <= nums[i] <= 10<sup>4</sup>
  * nums 为无重复元素的升序排列数组
  * -10<sup>4</sup> <= target <= 10<sup>4</sup>
  
## 解题思路
 * 标签：二分查找
 
 * 如果该题目暴力解决的话需要 O(n) 的时间复杂度，但是如果二分的话则可以降低到 O(logn) 的时间复杂度
 
 * 整体思路和普通的二分查找几乎没有区别，先设定左侧下标 left 和右侧下标 right，再计算中间下标 mid
 
 * 每次根据 nums[mid] 和 target 之间的大小进行判断，相等则直接返回下标，nums[mid] < target 则 left 右移，nums[mid] > target 则 right 左移
 
 * 查找结束如果没有相等值则返回 left，该值为插入位置
 
 * 时间复杂度：O(logn)
 
## 代码
 
 ```java

public class Solution {
    public int searchInsert(int[] nums, int target) {
         int left = 0, right = nums.length -1;
         while (left <= right) {
             int mid = left + (right -left) / 2;
             if (nums[mid] == target) {
                 return mid;
             } else if (nums[mid] < target){
                 left = mid + 1;
             } else {
                 right = right -1;
             }
         }
         return left;
    }
}
 ```
