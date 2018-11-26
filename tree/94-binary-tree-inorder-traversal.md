---
title: 94. 二叉树的中序遍历
date: 2018-11-24 23:10:02
tags: [树, 中等]
---

二叉树的三种遍历可直接递归实现，迭代实现需结合 `queue` 或 `stack` 来存储中间遍历结果。

<!-- more -->

## 题目描述

给定一个二叉树，返回它的*中序* 遍历。**示例:**

```
输入: [1,null,2,3]
   1
    \
     2
    /
   3
输出: [1,3,2]
```

**进阶:** 递归算法很简单，你可以通过迭代算法完成吗？

## 思路

作为 `tree` tag 的第一道题，先说下树的两个关键定义：

```go
// 树节点
type TreeNode struct {
	Val   int       // 节点存储的值
	Left  *TreeNode // 左子树的根
	Right *TreeNode // 右子树的根
}

// 树
type Tree struct {
	root *TreeNode // 整棵二叉树的根
}
```

一般的二叉树没什么插入的规律，只要满足每个节点的子节点不超过两个即可。在树章节默认使用 **BST** 二叉搜索树来解题。

### 插入

对于 BST 有直接的按大小插入操作：

```go
func (tree *Tree) insert(v int) {
	if tree.root == nil {
		tree.root = &TreeNode{Val: v}
		return
	}

	cur := tree.root
	for cur != nil {
		switch {
		case v < cur.Val:
			if cur.Left == nil {
				cur.Left = &TreeNode{Val: v} // bingo
				return
			}
			cur = cur.Left
		case v > cur.Val:
			if cur.Right == nil {
				cur.Right = &TreeNode{Val: v} // bingo
				return
			}
			cur = cur.Right
		default:
			return // 一般假设 BST 中元素不重复
		}
	}
}

func main() {
	tree := &Tree{}
	tree.root = &TreeNode{Val: 6}
	for _, v := range []int{2, 8, 1, 4, 7} {
		tree.insert(v)
	}
}

```

即可建立二叉树：

 ![image-20181125224526465](https://images.yinzige.com/2018-11-25-144527.png)



### 遍历

对于二叉树有 **3** 种遍历方式：

- 前序遍历（`Pre Order`）：对节点的处理先于两个儿子节点，**父节点 -> 左儿子 -> 右儿子**

  ![image-20181126204642033](https://images.yinzige.com/2018-11-26-124642.png)

- 中序遍历（`In Order`）：**左儿子 -> 父节点 -> 右儿子**

  ![image-20181126204614505](https://images.yinzige.com/2018-11-26-124615.png)

- 后序遍历（`Post Order`）：对节点的处理晚于两个儿子节点，**左节点 -> 右儿子 -> 父节点**

  ![image-20181126204544321](https://images.yinzige.com/2018-11-26-124545.png)

3 种遍历方式只是父节点处理时机不同，均可用递归实现。

### 中序遍历

顺序：`1 2 4 6 7 8`

 ![1](https://images.yinzige.com/2018-11-25-141433.gif)

实现：

```go
func preorderTraversal(node *TreeNode) {
	if node == nil { // 遍历递归退出条件，必不可少
		return
	}
	if node.Left != nil {
		preorderTraversal(node.Left)
	}
	fmt.Println(node.Val)
	if node.Right != nil {
		preorderTraversal(node.Right)
	}
}
```

树的遍历区别在于：父节点的处理时机与子节点时机不同。



### 前序遍历

```go
func inorderTraversal(node *TreeNode) {
	if node == nil {
		return
	}
	fmt.Println(node.Val)
	if node.Left != nil {
		inorderTraversal(node.Left)
	}
	if node.Right != nil {
		inorderTraversal(node.Right)
	}
}
```



### 后序遍历

```go
func postorderTraversal(node *TreeNode) {
	if node == nil {
		return
	}
	if node.Left != nil {
		postorderTraversal(node.Left)
	}
	if node.Right != nil {
		postorderTraversal(node.Right)
	}
	fmt.Println(node.Val)
}
```



## 实现

### 递归实现

上边的遍历直接输出，题要求存储在 `slice` 中，递归间传递数据使用 `return` 值或直接传递指针参数即可：

```go
func inorderTraversal(root *TreeNode) []int {
    var res []int
    traverse(root, &res)
    return res
}

func traverse(node *TreeNode, res *[]int) {
    if node == nil {
        return
    }
    if node.Left != nil {
        traverse(node.Left, res)
    }
    *res = append(*res, node.Val) // 存储遍历值
    if node.Right != nil {
        traverse(node.Right, res)
    }
}
```

执行耗时：**0 ms (100%)**



### 迭代实现

二叉树的遍历处理操作，不是递归就是结合队列、栈等结构实现，如先序遍历使用队列，而中序遍历使用栈：

```go
func inorderTraversal(root *TreeNode) []int {
	if root == nil {
		return nil
	}

	var res []int
	cur := root
	var stack []*TreeNode
	for len(stack) > 0 || cur != nil {
		if cur != nil {
			stack = append(stack, cur) // 尽可能把左边的节点先入栈
			cur = cur.Left
		} else {
			parent := stack[len(stack)-1] // 回溯处理节点值（左子节点和父节点）
			stack = stack[:len(stack)-1]
			res = append(res, parent.Val)
			cur = parent.Right // 处理右子节点
		}
	}

	return res
}
```

执行耗时：**0 ms (100%)**



## 最佳实现

递归 ok，迭代也 ok



## 总结

三种遍历的递归实现易懂，若迭代实现则需要借助 `queue` 和 `stack` 来存储中间结果。