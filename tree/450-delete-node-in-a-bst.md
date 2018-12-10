---
title: 450. 删除二叉搜索树中的节点
date: 2018-12-10 10:34:15
tags: [树, 中等]
---
在 BST 中删节点分 3 种情况讨论，当有两个子节点时向上或向下取最接近的值来替换。

<!-- more -->

### 题目描述

给定一个二叉搜索树的根节点 **root** 和一个值 **key**，删除二叉搜索树中的 **key** 对应的节点，并保证二叉搜索树的性质不变。返回二叉搜索树（有可能被更新）的根节点的引用。一般来说，删除节点可分为两个步骤：

1. 首先找到需要删除的节点；
2. 如果找到了，删除它。

**说明：** 要求算法时间复杂度为 O(h)，h 为树的高度。

**示例:**

```
root = [5,3,6,2,4,null,7]
key = 3

    5
   / \
  3   6
 / \   \
2   4   7

给定需要删除的节点值是 3，所以我们首先找到 3 这个节点，然后删除它。
一个正确的答案是 [5,4,6,2,null,null,7], 如下图所示：
    5
   / \
  4   6
 /     \
2       7

另一个正确答案是 [5,2,6,null,4,null,7]：
    5
   / \
  2   6
   \   \
    4   7
```



### 思路与实现

删除 BST 中的一个节点 N，可分节点情况讨论：

比如有二叉树：

![image-20181210111722486](https://images.yinzige.com/2018-12-10-031723.png)

- N 是叶子节点：直接删除即可（让 N 的父节点指向 N 的指针置空）

- N 有一个子节点：将子节点略过 N 嫁接到父节点上。假设删除 9

  ![image-20181210111811001](https://images.yinzige.com/2018-12-10-031811.png)

- N 有两个子节点：从左子树选取最大的节点，或从右子树选取最小的节点代替它。假设删除 6

  ![image-20181210111852506](https://images.yinzige.com/2018-12-10-031853.png)

可直接递归实现：

```go
func deleteNode(root *TreeNode, key int) *TreeNode {
	if root == nil {
		return nil
	}

	switch {
	case key < root.Val:
		root.Left = deleteNode(root.Left, key)
	case key > root.Val:
		root.Right = deleteNode(root.Right, key)
	default:
		// 只有一个子节点，直接删除
		if root.Left == nil {
			return root.Right
		} else if root.Right == nil {
			return root.Left
		}

		// 有两个子节点，找到值向上最接近的节点，替换掉
		root.Val = ceil(root.Right)
		root.Right = deleteNode(root.Right, root.Val) // 在右子树中删除该节点
	}

	return root
}

// 查找右子树上的最左节点
func ceil(root *TreeNode) int {
	for root.Left != nil {
		root = root.Left
	}
	return root.Val
}

// 也可以查找左子树的最有节点
```

**404 ms, faster than 52.94%**

依旧使用 [1, 2, 3] 最简单情形来调试。



### 最佳实现

如上的递归。



### 总结

参考：[Recursive Easy to Understand Java Solution](https://leetcode.com/problems/delete-node-in-a-bst/discuss/93296/Recursive-Easy-to-Understand-Java-Solution)