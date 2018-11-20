---
title: 35. 搜索插入位置
date: 2018-11-20 21:31:36
tags: [数组, 简单]
---

二分搜索结束后，若未查找到元素，双指针与 target 的位置关系：`r, [target], l`

<!-- more -->

### 题目描述

给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。你可以假设数组中无重复元素。**示例：**

```
输入: [1,3,5,6], 5
输出: 2

输入: [1,3,5,6], 2
输出: 1
```



### 思路与实现

二分搜索未找到 `target` 时，前后指针存在规律：`…, l, [target], r, …`，直接写二分搜索：

```go
func searchInsert(nums []int, target int) int {
	return binarySearch(nums, 0, len(nums)-1, target)
}

func binarySearch(nums []int, l, r int, target int) int {
	if l > r {
		return l // 递归退出时三数位置关系： r,[target],l
		// return r+1	// also ok
	}

	mid := (l + r) / 2
	switch {
	case target < nums[mid]:
		return binarySearch(nums, l, mid-1, target)
	case nums[mid] < target:
		return binarySearch(nums, mid+1, r, target)
	default:
		return mid
	}
}
```

执行耗时： **4 ms (100%)**



### 最佳实现
如上



### 总结

二分搜索，每次能排除 `target` 一定不存在的另一半，高效有用，33 34 35 有助于理解。