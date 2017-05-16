---
title: 【Leetcode】268.MissingNumber
date: 2016-03-16 15:57:01
tags: 
- 算法
- LeetCode
categories: 算法
---


## [题目](https://leetcode.com/problems/missing-number/) ##

> Given an array containing n distinct numbers taken from 0, 1, 2, ..., n, find the one that is missing from the array.

> For example,
Given nums = [0, 1, 3] return 2.

> Note:
Your algorithm should run in linear runtime complexity. Could you implement it using only constant extra space complexity?

## 思路 ##
> 先排序，调用标准库中的qsort快排函数
> 
> 判断相邻两个数字的差是否为1


## C代码 ##
	int comp(const void*a,const void*b)
	{
	    return *(int*)a-*(int*)b;
	}
	
	int missingNumber(int* nums, int numsSize) {
	    qsort(nums,numsSize,sizeof(int),comp);
	    if(numsSize>nums[numsSize-1]){
	        return numsSize;
	    }
	    int missing = -1;
	
	    int i,j;
	    int temp=-1;
	    for(i=0;i<numsSize;i++){
	        if((nums[i]-temp)!=1){
	            missing = nums[i]-1;
	            return missing;
	        }
	        temp = nums[i];
	    }
	
	    return 1;
	}

