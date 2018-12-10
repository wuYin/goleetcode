---
title: 543. 二叉树的直径
date: 2018-12-10 16:35:04
tags: [树, 简单]
---
求树深度时记录最大深度和，注意直径的中点不一定是根节点。

<!-- more -->

### 题目描述

给定一棵二叉树，你需要计算它的直径长度。一棵二叉树的直径长度是任意两个结点路径长度中的最大值。这条路径可能穿过根结点。

**示例 :**
给定二叉树

```
          1
         / \
        2   3
       / \     
      4   5    
```

返回 **3**, 它的长度是路径 [4,2,1,3] 或者 [5,2,1,3]。

**注意：**两结点之间的路径长度是以它们之间边的数目表示。



### 思路与实现

所谓节点 N 的直接，是指左子树的最长路径加右子树最长路径。这里不能一昧认为 N 就是根节点。比如：

![image-20181210170933140](https://images.yinzige.com/2018-12-10-090933.png)

这里的直径其实是左右深度之和，在求深度时比较记录最大和即可：

```go
func diameterOfBinaryTree(root *TreeNode) int {
	if root == nil {
		return 0
	}

	var maxDepth int
	var depth func(node *TreeNode) int
	depth = func(node *TreeNode) int {
		if node == nil {
			return 0
		}
		
		l := depth(node.Left)
		r := depth(node.Right)
		if l+r > maxDepth {
			maxDepth = l + r // 求深度的时候顺带求最长路径 // 边数不加1
		}
		
		if l > r {
			return l + 1
		}
		return r + 1
	}
	depth(root)

	return maxDepth
}
```

**12 ms, faster than 31.91%**



### 最佳实现

简介如上



### 总结

直径偷换了深度和的概念，理解题表达的意思注意配图的 case 即可。