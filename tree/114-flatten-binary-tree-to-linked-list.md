---
title: 114. 二叉树展开为链表
date: 2018-12-06 22:02:58
tags: [树, 中等]
---
遍历将左子树并入右子树，最终合并为右斜树。

<!-- more -->

### 题目描述

给定一个二叉树，[原地](https://baike.baidu.com/item/%E5%8E%9F%E5%9C%B0%E7%AE%97%E6%B3%95/8010757)将它展开为链表。例如，给定二叉树

```
    1
   / \
  2   5
 / \   \
3   4   6
```

将其展开为：

```
1
 \
  2
   \
    3
     \
      4
       \
        5
         \
          6
```



### 思路与实现

所谓展开为链表，其实是将左子树并入右子树，最终使整棵二叉树为右斜树。并入后不要忘了将左子树置空：

```go
func flatten(root *TreeNode) {
	if root == nil {
		return
	}
	flatten(root.Left)
	flatten(root.Right)

	if root.Left != nil {
		tmp := root.Right
		cur := root.Left
		
		root.Right = cur // 嫁接左子树的根节点
		for cur.Right != nil {
			cur = cur.Right
		}
		cur.Right = tmp // 嫁接左子树的唯一叶子节点

		root.Left = nil // 将左子树置空
	}
}
```

**8 ms, faster than 100.00%**



### 最佳实现

如上的递归并入



### 总结

 `flatten` 可以是先序遍历，也可以是后序遍历处理

