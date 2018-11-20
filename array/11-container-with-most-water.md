---
title: 11. 盛最多水的容器
date: 2018-11-19 22:14:21
tags: [数组, 中等]
---

巧用双指针。

<!--more -->

### 题目描述

给定 *n* 个非负整数 *a*1，*a*2，...，*a*n，每个数代表坐标中的一个点 (*i*, *ai*) 。在坐标内画 *n* 条垂直线，垂直线 *i* 的两个端点分别为 (*i*, *ai*) 和 (*i*, 0)。找出其中的两条线，使得它们与 *x* 轴共同构成的容器可以容纳最多的水。

**说明：**你不能倾斜容器，且 *n* 的值至少为 2。

![image-20181119221604021](https://images.yinzige.com/2018-11-19-141605.png)



## 思路与实现

### 暴力遍历

很直观的做法，把所有面积求出来，取最大值即可，但复杂度为：`O(N^2)`

```go
func maxArea(height []int) int {
	max := 0
	for i, h1 := range height {
		for j, h2 := range height[i+1:] {
			area := (j + 1) * min(h1, h2) // 宽度为 j+1
			if max < area {
				max = area
			}
		}
	}

    return max
}
```

执行耗时：**716 ms (18%)**



### 双指针

问题比较迷惑的地方在于，要同时兼顾宽和高取到最大面积，考虑**短板效应**：水桶的容积总取决于最短的板，本题中两条线段围成的 `area` 取决于较短的线段。

使用双指针 `left` 和 `right` 分别指向数组头尾，每次取面积比较后，向高线段移动使得面积更大（向低线段移动面积只会更小）

```go
func maxArea(height []int) int {
	l, r := 0, len(height)-1
	max := 0

	for l <= r {
		w := r - l
		h := min(height[l], height[r])
		if max < w*h {
			max = w * h
		}

		if height[l] < height[r] {
			l++
		} else {
			r--
		}
	}

	return max
}
```

执行耗时：**16 ms (100%)**



## 最佳实现

如上



## 总结

双指针解决数组问题十分有效。一般有 2 种用法：

- `left`> 首尾指针 <`right` 

- `slow`> `fast`>> 快慢指针

在数组问题和两个方向相关时，不妨考虑双指针，先排序处理等