---
title: 39. 组合之和
date: 2018-12-02 13:10:51
tags: [数组, 中等]
---

与 96 题类似，当大问题直接解决较为复杂时，考虑将大问题分解为小问题解决后，再将解集组合。代码中很可能用到递归来实现回溯，可以从最简单的输入一步一步调试实现。

<!-- more-->

### 题目描述

给定一个**无重复元素**的数组 `candidates` 和一个目标数 `target` ，找出 `candidates` 中所有可以使数字和为 `target` 的组合。`candidates` 中的数字可以无限制重复被选取。**说明：**

- 所有数字（包括 `target`）都是正整数。
- 解集不能包含重复的组合。 

**示例:**

```
输入: candidates = [2,3,6,7], target = 7,
所求解集为:
[
  [7],
  [2,2,3]
]

输入: candidates = [2,3,5], target = 8,
所求解集为:
[
  [2,2,2,2],
  [2,3,3],
  [3,5]
]
```



### 思路与实现

本题给的 label 是回溯算法，对数组中的任一个数 `curNum`，在剩余数组中找和为 `target - curNum` 的组合即可：

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
	next := target - nums[0] // 下一个和
	switch {
	case next < 0: // 当前数比 target 还大，这个组合没戏
		return nil
	case next == 0: // bingo
		return [][]int{{nums[0]}}
	case next > 0:
		combines := dfs(nums, next) // 寻找子和的任意组合
		for _, combine := range combines {
			res = append(res, append(combine, nums[0])) // 将子和的任意组合添加到解中
		}
	}

	// 如上尝试了以 nums[0] 开头的组合解
	res = append(res, dfs(nums[1:], target)...) // 在后续数组中尝试和为 target 的组合
	return res
}

```

执行耗时：**8 ms (50%)**



### 最佳实现
还有另一种深度优先的解法，也是递归，但是代码比这个晦涩，忽略。



### 总结

若大问题可拆解为小问题，一般用回溯算法解决，配合递归把子问题的结果集组合起来，本题即是这样的做法。



### 其他

上一次更新是上周一了，这周抽空把树相关的题做完了一半，接下来十二月的计划就是把数组、链表、字符串、栈、队列和树相关的题解写了，顺带写写 math tag 的题目。

写题多了，感觉把思路转换为代码更容易，理解了一堆之前觉得高深的名词，这两个月感觉收获蛮多。希望自己再接再厉，多学习。

把压力转换为动力，加油。