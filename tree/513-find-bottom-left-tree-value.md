---
title: 513. 找树左下角的值
date: 2018-12-10 15:14:11
tags: [树, 中等]
---
每个深度取一个值即可。此外使用队列按层遍历二叉树时，注意两个子节点入队列的顺序。

<!-- more -->

### 题目描述

给定一个二叉树，在树的最后一行找到最左边的值。

**示例 1:**

```
输入:

    2
   / \
  1   3

输出:
1
```

**示例 2:**

```
输入:
        1
       / \
      2   3
     /   / \
    4   5   6
       /
      7

输出:
7
```

 **注意:** 您可以假设树（即给定的根节点）不为 **NULL**。



### 思路与实现

本题与 [199. 二叉树的右视图](https://goleetcode.io/2018/12/07/tree/199-binary-tree-right-side-view/) 是一道题，每个深度取一个值，先序遍历即可：

```go
func findBottomLeftValue(root *TreeNode) int {
	var lefts []int
	traverse(root, 0, &lefts)
	return lefts[len(lefts)-1]
}

func traverse(root *TreeNode, depth int, lefts *[]int) {
	if root == nil {
		return
	}

	traverse(root.Left, depth+1, lefts) // 先序遍历，尽可能地先处理左子节点
	traverse(root.Right, depth+1, lefts)

	if depth == len(*lefts) {
		*lefts = append(*lefts, root.Val) // 新来最左子节点
	}
}
```

**8 ms, faster than 100.00%**

当然，层级队列也可以：

```go
func findBottomLeftValue(root *TreeNode) int {
	q := []*TreeNode{root}

	for len(q) > 0 {
		root = q[0]
		q = q[1:]

		if root.Right != nil {
			q = append(q, root.Right) // 不管哪一层，左节点总是在右节点后入队列
		}
		if root.Left != nil {
			q = append(q, root.Left)
		}
	}

	return root.Val
}
```

**12 ms, faster than 62.07%**



### 最佳实现

先序遍历取左子节点的值。



### 总结

和最相关的问题一般和层级遍历有关系，本题如果层级遍历到二维数组再去最后一个数组的第一个值…那还不如按层级队列，每层都能保证各自的左子节点在右子节点之后入队列，使最后出队列的节点就是最左节点，同理交换入队列的顺序，最后出队列的节点就是最右子节点。