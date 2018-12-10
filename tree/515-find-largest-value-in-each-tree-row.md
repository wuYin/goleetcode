---
title: 515. 在每个树行中找最大值
date: 2018-12-10 15:31:44
tags: [树, 中等]
---
与层序遍历相关的操作，可转换为 `depth` & `DFS`，也可转换为 `queue` & `BFS`

<!-- more -->

### 题目描述

您需要在二叉树的每一行中找到最大的值。

**示例：**

```
输入: 
          1
         / \
        3   2
       / \   \  
      5   3   9 

输出: [1, 3, 9]
```

### 思路与实现

和 513、199 一样都是层级遍历问题，遍历并比较最大值即可：

```go
func largestValues(root *TreeNode) []int {
   var maxs []int
   traverse(root, 0, &maxs)
   return maxs
}

func traverse(root *TreeNode, depth int, maxs *[]int) {
   if root == nil {
      return
   }

   if depth == len(*maxs) {
      *maxs = append(*maxs, root.Val)
   }
   if depth < len(*maxs) && root.Val > (*maxs)[depth] { // 存储该层的最大值
      (*maxs)[depth] = root.Val
   }

   traverse(root.Left, depth+1, maxs)
   traverse(root.Right, depth+1, maxs)
}
```

**12 ms, faster than 100.00%**



### 最佳实现

有 DFS 就可以试试 BFS：

```go
func largestValues(root *TreeNode) []int {
	if root == nil {
		return nil
	}

	var maxs []int
	q := []*TreeNode{root}
	for len(q) > 0 {
		levelMax := -1 << 31 // 起点
		count := len(q)

		for i := 0; i < count; i++ { // 按层遍历
			cur := q[0]
			q = q[1:]
			if cur.Val > levelMax {
				levelMax = cur.Val
			}

			if cur.Left != nil {
				q = append(q, cur.Left)
			}
			if cur.Right != nil {
				q = append(q, cur.Right)
			}
		}
		maxs = append(maxs, levelMax)
	}

	return maxs
}
```

**12 ms, faster than 100.00%**



### 总结

层序遍历相关的问题可以 DFS，也可以 Queue BFS，起源于：[102. 二叉树的层次遍历](https://goleetcode.io/2018/12/04/tree/102-binary-tree-level-order-traversal/)