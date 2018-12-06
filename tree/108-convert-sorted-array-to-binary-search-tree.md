---
title: 108. 将有序数组转换为二叉搜索树
date: 2018-12-06 15:03:43
tags: [树, 简单]
---
取中向左向右递归建树。

<!-- more -->

### 题目描述

将一个按照升序排列的有序数组，转换为一棵高度平衡二叉搜索树。本题中，一个高度平衡二叉树是指一个二叉树*每个节点* 的左右两个子树的高度差的绝对值不超过 1。

**示例:**

```
给定有序数组: [-10,-3,0,5,9],

一个可能的答案是：[0,-3,9,-10,null,5]，它可以表示下面这个高度平衡二叉搜索树：
      0
     / \
   -3   9
   /   /
 -10  5
```



### 思路与实现

高度平衡即需要尽量保持左右子树的高度差在 1 以内，刚好取数组中点为树的 root 节点：

```go
func sortedArrayToBST(nums []int) *TreeNode {
	if len(nums) == 0 {
		return nil
	}
	mid := len(nums) / 2

	return &TreeNode{
		Val:   nums[mid],
		Left:  sortedArrayToBST(nums[:mid]),
		Right: sortedArrayToBST(nums[mid+1:]),
	}
}
```

**176 ms, faster than 63.54% ** 



### 最佳实现

如上的取中递归建树。



### 总结

递归分析在二叉树处理中十分常见，分析清楚递归退出条件。