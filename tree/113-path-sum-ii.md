---
title: 113. 路径总和 II
date: 2018-12-06 17:08:27
tags: [树, 中等]
---
遍历记录路径时需对 `curPath` 进行 `copy` 操作，否则递归函数传参 slice 将在多处被修改。

<!-- more -->

### 题目描述

给定一个二叉树和一个目标和，找到所有从根节点到叶子节点路径总和等于给定目标和的路径。

**示例:** 给定如下二叉树，以及目标和 `sum = 22`

```
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \    / \
        7    2  5   1
```

返回:

```
[
   [5,4,11,2],
   [5,8,4,5]
]
```



### 思路与实现

与 [112. 路径总和](https://goleetcode.io/2018/12/06/tree/112-path-sum/) 相比，找到路径时需返回该路径。那遍历时记录路径即可：

```go
func pathSum(root *TreeNode, sum int) [][]int {
	var paths [][]int
	traverse(root, sum, nil, &paths)
	return paths
}

func traverse(node *TreeNode, target int, curPath []int, paths *[][]int) {
	if node == nil {
		return
	}

	// 酷酷的 copy 操作
	curPath = append(append([]int{}, curPath...), node.Val)
	if node.Left == nil && node.Right == nil && node.Val == target { // got one, bingo
		*paths = append(*paths, curPath)
		return
	}

	target -= node.Val
	traverse(node.Left, target, curPath, paths)
	traverse(node.Right, target, curPath, paths)
}
```

**12 ms, faster than 33.33%**



### 最佳实现

思路同上，代码细节可以再优化。



### 总结

slice 的 copy 操作真是酷：

```go
b = make([]T, len(a))
copy(b, a) // ok

b = append([]T(nil), a...) // best

b = append(a[:0:0], a...)  // better 
```

