---
title: 230. 二叉搜索树中第K小的元素
date: 2018-12-07 16:35:42
tags: [树, 中等]
---
对二叉树的 `stack` 中序遍历进行计数改造即可。

<!-- more -->

### 题目描述

给定一个二叉搜索树，编写一个函数 `kthSmallest` 来查找其中第 **k** 个最小的元素。可假设 k 总是有效：1 ≤ k ≤ 二叉搜索树元素个数。

**示例 1:**

```
输入: root = [3,1,4,null,2], k = 1
   3
  / \
 1   4
  \
   2
输出: 1
```

**示例 2:**

```
输入: root = [5,3,6,2,4,null,null,1], k = 3
       5
      / \
     3   6
    / \
   2   4
  /
 1
输出: 3
```



### 思路与实现

BST 的中序遍历结果便对应着 BST 的有序序列，改造一下迭代版的中序遍历即可：

```go
func kthSmallest(root *TreeNode, k int) int {
	if root == nil {
		return -1
	}

	var stack []*TreeNode
	cur := root
	for len(stack) > 0 || cur != nil {
		if cur != nil {
			stack = append(stack, cur) // 走到左子树的尽头
			cur = cur.Left
			continue
		}

		cur = stack[len(stack)-1] // 回溯处理
		stack = stack[:len(stack)-1]
		k--
		if k == 0 {
			return cur.Val // bingo
		}
		cur = cur.Right // 处理右子树
	}

	return -1
}
```

**16 ms, faster than 100.00%** 



### 最佳实现

如上的 stack 计数中序遍历。



### 总结

二叉树的前中后序遍历都可以 DFS 递归，也可以 Stack 迭代实现，各有应用场景。