---
title: 33. 搜索旋转排序数组
date: 2018-11-20 10:06:30
tags: [数组, 中等]
---

Binary Search 在涉及有序数组及其变种的问题中应用很多，但在排除另一半范围时要梳理好边界条件。

<!-- more -->

### 题目描述

假设按照升序排序的数组在预先未知的某个点上进行了旋转。例如数组 `[0,1,2,4,5,6,7]` 可能变为 `[4,5,6,7,0,1,2]` 

搜索一个给定的目标值，如果数组中存在这个目标值，则返回它的索引，否则返回 `-1` 。你可以假设数组中不存在重复的元素。你的算法时间复杂度必须是 *O*(log *n*) 级别。**示例:**

```
输入: nums = [4,5,6,7,0,1,2], target = 0
输出: 4

输入: nums = [4,5,6,7,0,1,2], target = 3
输出: -1
```



### 思路与实现

看到 `O(logN)` 和有序数组，二分搜索是跑不了的。实现：

```go
// 类二分查找
func search(nums []int, target int) int {
	l, r := 0, len(nums)-1

	for l <= r {
		mid := (l + r) / 2

		switch {
		case nums[mid] == target: // bingo
			return mid
		case nums[l] <= nums[mid]: // 左半部分有序
			if nums[l] <= target && target < nums[mid] { // 注意 <= 比较
				r = mid - 1 // 排除右半边
			} else {
				l = mid + 1 // 排除左半边
			}
		case nums[mid] < nums[r]: // 右侧有序
			if nums[mid] < target && target <= nums[r] {
				l = mid + 1
			} else {
				r = mid - 1
			}
		}
	}

	return -1
}
```

执行耗时：**4 ms (100%)**

<div class="tip">
    特别注意 switch 语句中 3 个 case 的顺序，及各自判断条件，特别是 "<="
</div>



### 最佳实现

如上



### 总结

二分搜索在有序数组及其变种问题中应用得很多，`O(logN)` 与二分一家亲，后边的有序数组插入位置问题也一样，梳理好边界条件后再排除另一半范围。