---
title: 34. 在排序数组中查找元素的第一个和最后一个位置
date: 2018-11-20 11:36:55
tags: [数组, 中等]
---

微调二分搜索，遇到 `target` 不停歇继续向左，向右取边界。

<!-- more -->

## 题目描述

给定一个按照升序排列的整数数组 `nums`，和一个目标值 `target`。找出给定目标值在数组中的开始位置和结束位置。你的算法时间复杂度必须是 *O*(log *n*) 级别。如果数组中不存在目标值，返回 `[-1, -1]`。**示例：**

```
输入: nums = [5,7,7,8,8,10], target = 8
输出: [3,4]

输入: nums = [5,7,7,8,8,10], target = 6
输出: [-1,-1]
```



## 思路与实现

### 直接二分搜索

第一反应是二分搜索查找到元素后，分别向左和向右查找边缘即可：

```go
func searchRange(nums []int, target int) []int {
	mid := binarySearch(nums, 0, len(nums)-1, target)
	l, r := mid, mid

	// 寻找左边界
	for i := mid - 1; i >= 0; i-- {
		if nums[i] != target {
			break
		}
		l = i
	}

	// 寻找右边界
	for i := mid + 1; i < len(nums); i++ {
		if nums[i] != target {
			break
		}
		r = i
	}

	return []int{l, r}
}

func binarySearch(nums []int, l, r int, target int) int {
	if l > r {
		return -1
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

执行耗时：**12 ms (100%)**，复杂度为 `O(logN + N)`，若 nums 大部分值是 target，则复杂度 `O(N)` 不符合题意。



### 微调二分搜索

可对二分搜索进行微调，在第一次寻找到值不停下来，而是一直寻找边界值，2 个方向二分搜索的复杂度为 `O(logN) `级别。微调实现：

```go
// 改进的二分搜索
func searchRange2(nums []int, target int) []int {
	l := edgeBinSearch(nums, true, target)
	r := edgeBinSearch(nums, false, target)
	return []int{l, r}
}

// 搜索时在匹配到 target 之后依旧向边缘走当做没匹配到，注意 2 个边界条件
func edgeBinSearch(nums []int, leftest bool, target int) int {
	n := len(nums)
	l, r := 0, n-1
	for l <= r {
		mid := (l + r) / 2
		switch {
		case target < nums[mid]:
			r = mid - 1
		case target > nums[mid]:
			l = mid + 1
		default:
			if leftest {
				if mid == 0 || nums[mid] > nums[mid-1] { // 不再继续向左走的 2 个边界条件
					return mid
				}
				r = mid - 1 // 继续在左侧找
			} else {
				if mid == n-1 || nums[mid] < nums[mid+1] { // 不往右走了，取边界值
					return mid
				}
				l = mid + 1 // 继续在右侧找
			}
		}
	}
	return -1
}

```

执行耗时：**12 ms (100%)**

## 最佳实现

如上改进后的二分搜索。



## 总结

对 Binary Search 应该巧理解，巧应用。在遇到 `target` 后可直接 `return`，但在本题中，为了取到边界值，需要不断地向边界试探。