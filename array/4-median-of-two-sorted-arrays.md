---

title: 4. 寻找两个有序数组的中位数
date: 2018-11-19 11:56:38
tags: [数组, 困难]
---

## 题目描述

给定两个大小为 m 和 n 的有序数组 `nums1` 和 `nums2`。请你找出这两个有序数组的中位数，并且要求算法的时间复杂度为 O(log(m + n))。你可以假设 `nums1` 和 `nums2` 不会同时为空。示例:

```
nums1 = [1, 3]
nums2 = [2]
则中位数是 2.0

nums1 = [1, 2]
nums2 = [3, 4]
则中位数是 (2 + 3)/2 = 2.5
```



## 思路与实现

### 合并后取中位数

先像归并排序一样将两数组 `merge` 为一个有序数组，直接取中位数即可：

```go
func findMedianSortedArrays(nums1 []int, nums2 []int) float64 {
	res := merge(nums1, nums2)
	n := len(res)
	if len(res)%2 == 1 {
		return float64(res[n/2])
	} else {
		l, r := res[n/2-1], res[n/2]
		return float64(l+r) / 2
	}
}

// 合并 2 个有序数组
func merge(nums1, nums2 []int) []int {
	n, m := len(nums1), len(nums2)
	res := make([]int, 0, n+m)
	i, j := 0, 0
	for ; i < n && j < m; {
		switch {
		case nums1[i] < nums2[j]:
			res = append(res, nums1[i])
			i++
		case nums1[i] > nums2[j]:
			res = append(res, nums2[j])
			j++
		default:
			res = append(res, nums1[i], nums2[j])
			i++
			j++
		}
	}

	// 合并剩余数组
	if i < n {
		res = append(res, nums1[i:]...) // 注意退出合并循环后起始索引是 i 不是 i+1
	}
	if j < m {
		res = append(res, nums2[j:]...)
	}
	return res
}
```

执行耗时：**44ms (14.4%)**，`merge` 操作致使复杂度为 `O(N+M)`

`merge` 等同于 `nums := append(nums1, nums2…); sort.Ints(nums)`，这种做法最直观但也最辣鸡。



### 二分搜索

看到数组和 `O(logN)` 的搜索复杂度，自然该想到二分搜索：

循环实现：

```go
func binarySearch(nums []int, target int) int {
	l, r := 0, len(nums)-1
	for l <= r {
		mid := (l + r) / 2 // mid = l + (r-l)/2
		switch {
		case nums[mid] < target: // 在右侧
			l = mid + 1
		case target < nums[mid]: // 在左侧
			r = mid - 1
		default:
			return mid // got you
		}
	}
	return -1
}
```

递归实现：

```go
func binarySearch(nums []int, l, r int, target int) int {
	if l > r {
		return -1 // 必须有的递归退出条件
	}
	
	mid := (l + r) / 2
	switch {
	case nums[mid] < target:
		return binarySearch(nums, mid+1, r, target)
	case target < nums[mid]:
		return binarySearch(nums, l, mid-1, target)
	default:
		return mid
	}
}
```

二分搜索的本质是一次性排除当前搜索范围的一半，这一半不包含 target，本题便拓展为在双数组中查找第 K 小的数，此时的 `K = (m+n)/2`

先跳过数组为空，k == 0 等特殊情况，假定 `nums` 是合并数组，将 `nums1` 和 `nums2` 按 `k/2` 来切割：

 ![image-20181119144359418](https://images.yinzige.com/2018-11-19-064400.png)

- **V1 > V2**

  说明 section3 中所有的数都会都在 nums 合并数组中前 K 小的元素中，即 `nums2[0, k/2-1]` 是 `nums[0,k-1]` 的子集，section3 在后续搜索中被舍弃，在 `[section1, section2, section4]` 中搜索第 **K/2** 大的数。

- **V1 < V2**

  舍弃 section1，在 `[section1, section2, section4]` 中搜索第 **K/2** 大的数。

递归搜索结束条件：

- `K=0` 取区域1 和 区域3 中位置 0 上的较小者
- 其中一个数组为空，继续数到第 K 个元素（可做预处理，保证 a 数组短，b 数组长）

举个实在的例子：

```go
nums1 = []int{0, 1, 5, 8, 9, 12, 14}
nums2 = []int{2, 4, 10, 11, 13}
// 如何在 12 个数中找出第 K=7 小的数：9

// 先分区：K/2 = 7/2 =3
// [0,1,5,8]   [9,12,14]
// [2,4,10,11] [13]
// 发现 8<11 则舍弃区域1，问题从四个区域找第 7 个数变为在三个区域找第 3(7-4) 个数

// 再分区：K/2 = 3/2 = 1
// [9,12] [14]
// [2,4]  [10,11 13]
// 发现 12>4，舍弃区域3，在剩下三个区域中找第 1(3-2) 个数

// 再分区：K/2 = 1/2 = 0
// [9]  [12, 14]
// [10] [11 13]
// 取到小的数 9，bingo

// 至此，丢4个，丢2个，丢1个，最终找到了第 7 小的数：9
```

代码实现：

```go

func findMedianSortedArrays(a []int, b []int) float64 {
	n1, n2 := len(a), len(b)
	if (n1+n2)%2 == 1 {
		return float64(findKth(a, b, n1, n2, (n1+n2)/2+1)) // 注意 +1
	}
	l := findKth(a, b, n1, n2, (n1+n2)/2)
	r := findKth(a, b, n1, n2, (n1+n2)/2+1)
	return float64(l+r) / 2
}

// 关键操作
func findKth(a, b []int, n1, n2 int, k int) int {
	if n1 > n2 {
		n1, n2 = n2, n1
		a, b = b, a // 确保数组 a 短 b 长方便处理
	}

	if n1 == 0 {
		return b[k-1] // bingo
	}

	if k == 1 {
		return min(a[0], b[0]) // bingo
	}

	k1 := min(n1, k/2) // 避免短数组越界
	k2 := k - k1
	switch {
	case a[k1-1] < b[k2-1]:
		return findKth(a[k1:], b, len(a[k1:]), n2, k-k1) // 舍弃区域 1
	case a[k1-1] > b[k2-1]:
		return findKth(a, b[k2:], n1, len(b[k2:]), k-k2) // 舍弃区域 3
	default:
		return a[k1-1] // bingo
	}
}

func min(x, y int) int {
	if x < y {
		return x
	}
	return y
}
```

执行耗时：**28 ms (57%)**，由于每次操作都会剔除 `K/2` 的元素，K = (M+N)/2 时算法复杂度为 `log(M+N)`

性能最高的居然是 merge，这个 OJ 有点迷...



## 最佳实现

如上



## 总结

抽离本题的本质：在有序双数组中查找第 K 大的数。看到 `logN` 级别的有序数组搜索复杂度，类似二分是跑不了的，本质上还是和二分搜索一样，一次性排除另一 `k/2` 的元素，直到：

- a 为空，取 b[0] 即可
- k 为1，取 `min(a[0], b[0])` 即可。如例子中的 9 和 10
- 刚刚好，两个数组  k/2 处的值相等，取其一即可，bingo

<div class="tip">
注意处理代码中的数组越界，可以先做 a 短 b 长等预处理
</div>

