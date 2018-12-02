---
title: 40. 组合总和 II
date: 2018-12-02 15:48:49
tags: [数组, 中等]
---

添加 <u>一个数只能在一个组合中用一次</u> 的限制条件后，要进行去重处理。

<!-- more -->

### 题目描述

给定一个数组 `candidates` 和一个目标数 `target` ，找出 `candidates` 中所有可以使数字和为 `target` 的组合。`candidates` 中的每个数字在每个组合中只能使用一次。**说明：**

- 所有数字（包括目标数）都是正整数。
- 解集不能包含重复的组合。 

**示例:**

```
输入: candidates = [10,1,2,7,6,1,5], target = 8,
所求解集为:
[
  [1, 7],
  [1, 2, 5],
  [2, 6],
  [1, 1, 6]
]

输入: candidates = [2,5,2,1,2], target = 5,
所求解集为:
[
  [1,2,2],
  [5]
]
```



### 思路与实现

和上一题 39 很像，但条件从 <u>数组中的数可重复使用</u> 变为 <u>每个数在每个组合中只能使用 1 次</u>，在 39 中有：

```
输入 [1, 1, 3]
输出 [[1 1 1] [1 1 1] [2 1] [1 1 1] [2 1]]
```

在新条件下，不会再出现 `[1, 1, 1]` 的组合，需做两点修改：

- 寻找子和的组合时，不再包含当前值（当前值只能在当前组合中使用一次）
- 处理完子和后，需要对数组进行去重处理

```go
func combinationSum(candidates []int, target int) [][]int {
	sort.Ints(candidates)
	return dfs(candidates, target)
}

// 在数组 nums 中选取和为 target 的组合
func dfs(nums []int, target int) [][]int {
	if len(nums) == 0 {
		return nil
	}

	var res [][]int
	next := target - nums[0]
	switch {
	case next < 0:
		return nil
	case next == 0: // bingo
		return [][]int{{nums[0]}}
	case next > 0:
		combines := dfs(nums[1:], next) // 1 // 在 剩余的数组 中寻找子和的任意组合
		for _, combine := range combines {
			res = append(res, append(combine, nums[0]))
		}
	}

	// 2 // 处理数组，跳过重复元素，避免重复解
	for len(nums) >= 2 && nums[0] == nums[1] {
		nums = nums[1:]
	}

	// 如上尝试了以 nums[0] 开头的组合解
	res = append(res, dfs(nums[1:], target)...) // 在后续数组中尝试和为 target 的组合
	return res
}
```

执行耗时：**8 ms (75%)**



### 最佳实现
如上的回溯法。



### 总结

第 40 题相对于 39 题限制了元素的选取，则需要在回溯时去掉当前值，并跳过重复元素。