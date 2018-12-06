---
title: 105. 从前序与中序遍历序列构造二叉树
date: 2018-12-06 10:38:47
tags: [树, 中等]
---
前序的第一个元素的是 root，后序的最后一个元素是 root，再根据中序划分左右子树，递归分析建树。

<!-- more -->

### 题目描述

根据一棵树的前序遍历与中序遍历构造二叉树。**注意:**你可以假设树中没有重复的元素。例如，给出

```
前序遍历 preorder = [3,9,20,15,7]
中序遍历 inorder = [9,3,15,20,7]
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

105 和 106 其实是同一道题，根据前序和中序、中序和后序恢复树。先理清楚 3 种遍历对节点的处理顺序：

![image-20181125224526465](https://images.yinzige.com/2018-11-25-144527.png)

- 前序遍历：父节点 -> 左节点 -> 右节点：**<u>6</u>** 2 1 4 8 7
- 中序遍历：左节点 -> 父节点 -> 右节点：1 2 4 6 7 8（BST 的有序序列）
- 后序遍历：左节点 -> 右节点 -> 父节点：1 4 2 7 8 <u>**6**</u>

注意标出的根节点 6，在前序中是第一个值，在后序中是最后一个值。

回到本题，前序是 `[3, 9, 20, 15, 7]`，则根节点为 3。后序是 `[9, 3, 15, 20, 7]`，则根节点左子树的后序是 9，右子树的后序是 15, 20, 7，得到：

```
    3
   / \
  9  {20, 15, 7}
```

再聚焦到右子树上，它的前序是 `[20, 15, 17]`，显然右子树的根节点是 20 … 总结一下：

- 找到根节点，划分好左子树和右子树
- 对左子树进行递归分析
- 对右子树进行递归分析

代码实现：

```go
func buildTree(preorder []int, inorder []int) *TreeNode {
	if len(preorder) == 0 {
		return nil
	}

	root := &TreeNode{Val: preorder[0]} // 根节点
	var i, v int
	for i, v = range inorder {
		if v == root.Val {
			break // 划分左右子树
		}
	}

	leftTree := inorder[:i]
	rightTree := inorder[i+1:]

	// root [left tree] [right tree]
	root.Left = buildTree(preorder[1:1+len(leftTree)], leftTree)
	root.Right = buildTree(preorder[1+len(rightTree):], rightTree)
	return root
}
```

**32 ms, faster than 92.39%**



### 最佳实现

如上的递归建树。



### 总结

用代码实现建树的分析规律即可，注意递归的退出条件。

