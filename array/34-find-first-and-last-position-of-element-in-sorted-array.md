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
func searchRange(nums []int, target int) []int {
	edges := []int{-1, -1}
	n := len(nums)
	switch {
	case n <= 0:
		return edges // not ok
	case n == 1:
		if nums[0] == target {
			return []int{0, 0}
		} else {
			return edges // not ok
		}
	}

	l, r := 0, n-1
	for l < r {
		mid := (l + r) / 2
		if target == nums[mid] {
			edges[0] = mid
		}
		if target <= nums[mid] {
			r = mid
		} else {
			l = mid + 1 // l 向左边缘逼近
		}
	}

	if nums[l] == target {
		edges[0] = l // 注意处理此种 case: 退出搜索时 l 可能等于 r，同时可能携带边界值
	} else {
		return edges // not ok
	}

	l, r = 0, n-1
	for l < r {
		mid := (l + r) / 2
		if nums[mid] == target {
			edges[1] = mid
		}
		if target >= nums[mid] {
			l = mid + 1 // l 向右边缘逼近
		} else {
			r = mid
		}
	}
	if nums[l] == target {
		edges[1] = l // 处理边界值
	}

	return edges
}
```

执行耗时：**12 ms (100%)**

<div class="tip">
    注意 edges 两个元素的取值，以及退出搜索后对边界值的处理。
</div>


## 最佳实现

如上的微调二分搜索。



## 总结

对 Binary Search 应该巧理解，巧应用。在遇到 `target` 后可直接 `return`，但在本题中，为了取到边界值，需要不断地向边界试探。