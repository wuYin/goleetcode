---
title: 27. 移除元素
date: 2018-11-20 01:31:29
tags: [数组, 简单]
---

快慢指针很 ok

<!-- more -->

### 题目描述

给定一个数组 *nums* 和一个值 *val*，你需要**原地**移除所有数值等于 *val* 的元素，返回移除后数组的新长度。不要使用额外的数组空间，你必须在**原地修改输入数组**并在使用 O(1) 额外空间的条件下完成。元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。**示例 1**

```
nums = [3,2,2,3], val = 3,函数应该返回新的长度 2, 并且 nums 中的前两个元素均为 2。
nums = [0,1,2,2,3,0,4,2], val = 2, 函数应返回新长度 5, 且 nums 前五个元素为 0, 1, 3, 0, 4。
注意这五个元素可为任意顺序。你不需要考虑数组中超出新长度后面的元素。
```



### 思路与实现
和 26 题一模一样，快慢指针很 ok

```go
func removeElement(nums []int, val int) int {
	s, f := 0, 0
	for f < len(nums) {
		if nums[f] != val {
			nums[s] = nums[f]
			s++
		}
		f++
	}
	return s
}
```

执行耗时：**0 ms (100%)**

或者直接遇到 `val` 就真的 `remove`：

```go
func removeElement(nums []int, val int) int {
	for i := 0; i < len(nums); i++ {
		if nums[i] == val {
			nums = append(nums[:i], nums[i+1:]...) // 删除 nums[i]
			i--
		}
	}
	return len(nums)
}
```



### 最佳实现

如上



### 总结

做题，掌握核心科技。