---
title: 437. 路径总和 III
date: 2018-12-08 13:19:49
tags: [树, 简单]
---
遍历二叉树时计算 prefix sum，配合 map 值检查来计数路径。

<!-- more -->

### 题目描述

给定一个二叉树，它的每个结点都存放着一个整数值，找出路径和等于给定数值的路径总数。

路径不需要从根节点开始，也不需要在叶子节点结束，但是路径方向必须是向下的（只能从父节点到子节点）

**示例：**

```
root = [10,5,-3,3,2,null,11,3,-2,null,1], sum = 8

      10
     /  \
    5   -3
   / \    \
  3   2   11
 / \   \
3  -2   1

返回 3。和等于 8 的路径有:

1.  5 -> 3
2.  5 -> 2 -> 1
3.  -3 -> 11
```



### 思路与实现

先看前两道题：[112. 路径总和](https://goleetcode.io/2018/12/06/tree/112-path-sum/)，[113. 路径总和 II](https://goleetcode.io/2018/12/06/tree/113-path-sum-ii/)，可以对二叉树进行全遍历逐个 node 计数路径，合并一下节点遍历与路径和计算，有 DFS 解法：

```go
func pathSum(root *TreeNode, sum int) int {
	if root == nil {
		return 0
	}
	return find(root, sum) + pathSum(root.Left, sum) + pathSum(root.Right, sum) // 第一层 DFS
}

func find(root *TreeNode, target int) int {
	if root == nil {
		return 0
	}

	var count int
	if root.Val == target {
		count++ // 不返回，继续向下遍历
	}

	count += find(root.Left, target-root.Val) // 第二层 DFS
	count += find(root.Right, target-root.Val)
	return count
}
```

**20 ms, faster than 81.82%**

上边的两层 DFS 相当于暴力遍历，效率不高。



### 最佳实现

可参考 [17 ms O(n) java Prefix sum method](https://leetcode.com/problems/path-sum-iii/discuss/91878/17-ms-O(n)-java-Prefix-sum-method)， 类似 two sum 的解法。

比如遍历时有一条路径：`1 2 -1 -1 2`，对应的前缀和为 `1 3 2 1 3`，如下：

![image-20181209131621526](https://images.yinzige.com/2018-12-09-051622.png)

若 target sum 为 2，则遍历此路径时有：

- 走到都一个节点 1：前边没有和为 （1-2 = -1） 的节点，not ok
- 第二个节点 3：前边有 （3-2 = 1）的节点， 去掉第节点 1 有路径  `[2]`
- 第三个节点 2：ok，直接有路 `[1, 2, -1]`
- 第四个节点 1：前边没有值为 -1 节点，not ok
- 第五个节点 3：前边两个节点 1，会有 2 条路：`[2, -1, -1, 2]`, `[2]`

归纳：当遍历到值为 N 的节点，检查之前是否有 prefix sum 为 N - target 的节点，有的话从该节点到本节点的路径和恰为 target。检查是否存在使用 map 高效。

实现：

```go
func pathSum(root *TreeNode, sum int) int {
	m := map[int]int{0: 1}
	var counter int
	traverse(root, sum, 0, m, &counter)
	return counter
}

func traverse(root *TreeNode, target, curSum int, preSums map[int]int, counter *int) {
	if root == nil {
		return
	}

	curSum += root.Val
	if count, ok := preSums[curSum-target]; ok { // 有多少个就有多少条路 // 最后一个节点 3 之前有两个节点 1，就有两条路
		*counter += count
	}
	preSums[curSum]++

	traverse(root.Left, target, curSum, preSums, counter)  // 检查左子树上的路径
	traverse(root.Right, target, curSum, preSums, counter) // 检查右子树上的路径

	preSums[curSum]-- // 向上回溯时把前缀和去掉
}
```

**8 ms, faster than 100.00%**



### 总结

题目只要求路径计数，所以把 two sum 的 map 用法推广到二叉树时，有指定的节点值在前边的路出现时，就出现一条路径，这个思路十分地巧妙。