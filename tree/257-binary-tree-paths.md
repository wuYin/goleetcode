---
title: 257. 二叉树的所有路径
date: 2018-12-07 18:15:12
tags: [树, 简单]
---
112，113，257 是同一道题。

<!-- more -->

### 题目描述

给定一个二叉树，返回所有从根节点到叶子节点的路径。

**示例:**

```
输入:
   1
 /   \
2     3
 \
  5

输出: ["1->2->5", "1->3"]
解释: 所有根节点到叶子节点的路径为: 1->2->5, 1->3
```



### 思路与实现

本题与 [113. 路径总和 II](https://goleetcode.io/2018/12/06/tree/113-path-sum-ii/) 无差别：

```go
func binaryTreePaths(root *TreeNode) []string {
	var paths [][]int
	traverse(root, nil, &paths)
	
	res := make([]string, len(paths))
	for i, path := range paths {
		res[i] = strings.Trim(strings.Join(strings.Split(fmt.Sprint(path), " "), "->"), "[]") // cool code
	}
	
	return res
}

// 遍历记录路径
func traverse(root *TreeNode, curPath []int, paths *[][]int) {
	if root == nil {
		return
	}

	curPath = append(append([]int{}, curPath...), root.Val) // 匿名中间 slice 变量
	if root.Left == nil && root.Right == nil {
		*paths = append(*paths, curPath)
		return
	}
	
	traverse(root.Left, curPath, paths)
	traverse(root.Right, curPath, paths)
}
```

**4 ms, faster than 20.41%**



### 最佳实现

看 20.41% 就知道问题被我们搞复杂了，无需存储中间路径最后集中处理：

```go
func binaryTreePaths(root *TreeNode) []string {
	var paths []string
	traverse(root, "", &paths)
	return paths
}

// 遍历记录路径
func traverse(root *TreeNode, curPath string, paths *[]string) {
	if root == nil {
		return
	}

	curPath += strconv.Itoa(root.Val)
	if root.Left == nil && root.Right == nil {
		*paths = append(*paths, curPath) // bingo
		return
	}
	traverse(root.Left, curPath+"->", paths)
	traverse(root.Right, curPath+"->", paths)
}
```

**0 ms, faster than 100.00%**



### 总结

112，113，257 其实是一道题，都是在 DFS 求解路径。