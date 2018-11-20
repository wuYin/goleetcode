---
title: 16. 最接近的三数之和
date: 2018-11-20 00:26:00
tags: [数组, 中等]
---

预排序与双指针搞定遍历问题。

<!--more -->

### 题目描述

给定一个包括 *n* 个整数的数组 `nums` 和 一个目标值 `target`。找出 `nums` 中的三个整数，使得它们的和与 `target` 最接近。返回这三个数的和。假定每组输入只存在唯一答案。

```
例如，给定数组 nums = [-1，2，1，-4], 和 target = 1
与 target 最接近的三个数的和为 2. (-1 + 2 + 1 = 2).
```



### 思路与实现

这与上一题 3sum 十分地像，不过求的是**最接近**，即差的绝对值最小。依旧双指针跑不了的：

```go
func threeSumClosest(nums []int, target int) int {
	sort.Ints(nums)
	minSum := nums[0] + nums[1] + nums[2] // 初始值就得取前三个之和
	minAbs := abs(minSum - target)

	for i := 0; i < len(nums); i++ {
		if i > 0 && nums[i] == nums[i-1] {
			continue // 依旧遍历数去重，必需的
		}

		l, r := i+1, len(nums)-1
		for l < r {
			sum := nums[i] + nums[l] + nums[r]
			if minAbs > abs(sum-target) {
				minAbs = abs(sum - target)
				minSum = sum
			}

			if sum < target {
				l++ // 和小了，向后走更大
			} else {
				r-- // 和大了，向前走更小
			}
		}
	}
	return minSum
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

执行耗时：**8 ms (70%)**



### 最佳实现

如上



### 总结

预排序配合双指针遍历，很 ok

<div class="tip">
注意边界条件：双指针是否能相遇，即退出条件：若允许重复值则是 l<=r，若不允许则为 l<r
</div>

