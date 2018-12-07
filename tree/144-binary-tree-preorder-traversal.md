---
title: 144. 二叉树的前序遍历
date: 2018-12-07 11:11:58
tags: [树, 中等]
---
stack 向左迭代时 Push 右子节点，向上回溯时再 Pop 处理。

<!-- more -->

### 题目描述

给定一个二叉树，返回它的 *前序* 遍历。

 **示例:**

```
输入: [1,null,2,3]  
   1
    \
     2
    /
   3 

输出: [1,2,3]
```

**进阶:** 递归算法很简单，你可以通过迭代算法完成吗？



### 思路与实现

递归简单高效：

```go
func preorderTraversal(root *TreeNode) []int {
	var res []int
	traverse(root, &res)
	return res
}

func traverse(root *TreeNode, res *[]int) {
	if root == nil {
		return
	}
	*res = append(*res, root.Val)
	traverse(root.Left, res)
	traverse(root.Right, res)
}
```

**0 ms, faster than 100.00%**

这里 LeetCode 的 Golang OJ 有个复用全局变量的问题，比如我想简化代码：

```go
var res []int

func preorderTraversal(root *TreeNode) []int {
	if root == nil {
		return nil
	}

	res = append(res, root.Val)
	preorderTraversal(root.Left)
	preorderTraversal(root.Right)

	return res
}
```

第一个 case 的树是 `[1,null,2,3]  `， 第二个是 `[1]`，然后就 wrong answer：

![image-20181207112229502](https://images.yinzige.com/2018-12-07-032230.png)



### 最佳实现

使用栈实现的迭代前序遍历：

- 尽可能地向左走，并将右子节点依次入栈
- 当左子树遍历完后，回溯向上一层层地出栈处理右子节点

![pre](https://images.yinzige.com/2018-12-07-042337.gif)

GIF 来源：[techieme.in](http://techieme.in/postorder-node-iterator-of-binary-tree/)

代码实现：

```go
func preorderTraversal(root *TreeNode) []int {
	var res []int
	var stack []*TreeNode

	cur := root
	for cur != nil {
		res = append(res, cur.Val) // 处理根节点
		if cur.Right != nil {
			stack = append(stack, cur.Right)
		}
		cur = cur.Left // 处理左子节点
		if cur == nil && len(stack) > 0 {
			cur = stack[len(stack)-1] // 左子树处理完毕，回溯向上处理右子节点
			stack = stack[:len(stack)-1]
		}
	}
	
	return res
}
```

**0 ms, faster than 100.00%**



### 总结

栈操作的是向左走路上的右子节点，回溯时栈的 LIFO 特性正好是 root -> left -> right 的操作。