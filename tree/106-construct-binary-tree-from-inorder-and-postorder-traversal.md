---
title: 106. 从中序与后序遍历序列构造二叉树
date: 2018-12-06 11:30:52
tags: [树, 中等]
---
分析遍历序列的规律来递归建树。

<!-- more -->

### 题目描述

根据一棵树的中序遍历与后序遍历构造二叉树。**注意：**你可以假设树中没有重复的元素。例如，给出

```
中序遍历 inorder = [9,3,15,20,7]
后序遍历 postorder = [9,15,7,20,3]
```

返回如下的二叉树：

```
    3
   / \
  9  20
    /  \
   15   7
```



### 思路与实现

和上一题 105 一样，递归分析即可：

```go
func buildTree(inorder []int, postorder []int) *TreeNode {
	if len(inorder) == 0 {
		return nil
	}

	root := &TreeNode{Val: postorder[len(postorder)-1]}
	var i, v int
	for i, v = range inorder {
		if v == root.Val {
			break // 划分左右子树
		}
	}

	leftTree := inorder[:i]
	rightTree := inorder[i+1:]

	// [left tree] [right tree] root
	root.Left = buildTree(leftTree, postorder[:len(leftTree)])
	root.Right = buildTree(rightTree, postorder[len(leftTree):len(postorder)-1])
	return root
}
```

**32 ms, faster than 93.33%**



### 最佳实现

如上的递归分析。



### 总结

做完 105，106 可知由前序和中序，或中序和后序都能唯一确定一棵二叉树。但是由前序和后序来确定一棵二叉树，就需要分节点的 <u>**度**</u> 讨论。

节点的度是指节点的直接儿子个数，即它的子树有多少棵。

现分 3 种最简单的情况讨论：

- 二叉树中节点的度都为 0：只有一个节点即根节点，ok

- 二叉树中节点的度不是 0 就是 2：能还原，ok

  例：假设要恢复如下的二叉树，已知前序为 `[A, B, C]`，后序为 `[B, C, A]`

  ```
    A
   / \
  B   C
  ```

  - 由前序或后序都能知道 root 是 A
  - 根据前序知道 B C 无非三种情况：BC 一左一右，BC 都在左子树，BC 都在右子树
    - B 和 C 都在左子树或右子树，B 都是 C 的父节点，那后序遍历应该是 `[C, B]`，与已知矛盾
    - B 和 C 一左一右，根据后序可知 B 为左节点，C 为右节点，bingo

  所以当二叉树的任意节点度为 0 或 2 时，也能唯一确定一棵二叉树。

- 二叉树中有的节点度为 1，not ok

  例：假设要恢复如下的二叉树，已知前序为 `[A, B]`，后序为 `[B, A]`

  ```
    A
   /
  B  
  ```

  由前序知道根为 A，由后序知道根为 B，恢复如下：

  ```
    A  B
   /    \
  B      A
  ```


可递归地推广到整个二叉树的还原中，已知前序和后序：

- 若二叉树中所有节点的度不是 0 就是 2 （有点满二叉树的味道），可完整还原
- 若有节点度为 1，则无法确定根节点，无法还原