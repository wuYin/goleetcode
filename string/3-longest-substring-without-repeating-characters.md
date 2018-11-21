---
title: 3. 无重复字符的最长子串
date: 2018-11-21 22:43:15
tags: [字符串, 中等]
---

线性查找哈希表，滑动窗口计算最大不重复距离。

<!-- more -->

### 题目描述

给定一个字符串，请你找出其中不含有重复字符的 **最长子串** 的长度。**示例：**

```
输入: "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。

输入: "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```



### 思路与实现
自己假设更为一般的 case: `abcabd`，在遇到第二个 `a` 时最长子串从 `abc` 更新为了 `bca`，接着从 `bca` 更新为了 `cab`，最后发现最长子串 `cabd`

类比 TCP 做流量控制的滑动窗口，向后遍历时若遇到重复字符，则丢弃重复字符之前的部分，并记录最大子串长即可：

```go
func lengthOfLongestSubstring(s string) int {
	m := make(map[rune]int) // 去重 map

	maxLen := 0
	start := -1 // 指向当前不重复子串起始位置
	for end, r := range s {
		if last, ok := m[r]; ok && start < last { // 字符再次出现
			start = last // start 向前滑动
		}

		if end-start > maxLen {
			maxLen = end - start
		}

		m[r] = end // recording...
	}

	return maxLen
}
```

执行耗时：**8 ms (87%)**



### 最佳实现

如上的滑动窗口。



### 总结

这道题与 [two sum](https://goleetcode.io/2018/11/18/array/1-two-sum/) 十分地像，查找指定值是否存在于某个集合，查询复杂度为 `O(1)` 是个好选择。此外，本题滑动窗口中 `start` 的设计也比较巧妙，找到再次出现的字符 `A2` 时，子串需舍弃 `A1` 前的部分，并计算最大长度。