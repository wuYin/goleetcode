---
title: 5. 最长回文子串
date: 2018-11-23 12:17:36
tags: [字符串, 中等]
---

对每个字符建立回文半径，大回文子串内的字符初始半径不是 1，而是它最有可能的大回文半径，这就是 Manacher 算法的高效之处。

## 题目描述

给定一个字符串 `s`，找到 `s` 中最长的回文子串。你可以假设 `s` 的最大长度为 1000。**示例：**

```
输入: "babad"
输出: "bab"
注意: "aba" 也是一个有效答案。

输入: "cbbd"
输出: "bb"
```



## 思路与实现
### O(N^3) 遍历

拆解题意：子串 + 回文，最直接的做法：取所有子串，判断是否为回文。实现：

```go
func longestPalindrome(s string) string {
	runes := []rune(s)
	n := len(runes)
	if n <= 1 {
		return s
	}

	maxLen := 0
	maxL, maxR := 0, 0
	for i := 0; i < n; i++ {
		for j := i + 1; j < n; j++ {
			// 向后取所有子字符串，并判断回文及其长度
			// 复杂度为 O(N^2)
			if isPalindrome(runes[i:j+1]) && j-i+1 > maxLen {
				maxLen = j - i + 1 // i 到 j 的距离为 j-i+1
				maxL, maxR = i, j
			}
		}
	}
	return string(runes[maxL : maxR+1]) // 若开头不处理空串，注意此处空字符串会转换为 "\u0000"
}

// O(N) 判断是否为回文字符串
func isPalindrome(runes []rune) bool {
	l, r := 0, len(runes)-1
	for l <= r {
		if runes[l] != runes[r] {
			return false
		}
		l++
		r--
	}
	return true
}
```

执行耗时：**812 ms (10%)**，取子串和判断是否为回文总体复杂度为 `O(N^3)`，不慢才怪。

注意 `[]rune` 转 `string` 时的小问题：

```go
func main() {
	s := ""
	runes := []rune(s)
	fmt.Printf("%q\n", string(runes))      // ""
	fmt.Printf("%q\n", string(runes[0:1])) // "\x00"
}
```

字符串是以 `"\0"` 结尾的字节数组，把空字节 `0` 转为字符串时值为 `\x00`



### O(N^2) 遍历

细想一下，取子串与判断回文其实是可以结合在一起的。对某个字符，分别向左和向右扩张，遇到的字符对称相等则为回文串，直到扩张到不相等的边界。

注意：`aba` 与 `acca` 向外扩张的出发点不一样，前者的左右出发点重叠为一个点 `b`，后者没有重叠都是 `c`

```go
func longestPalindrome(s string) string {
	runes := []rune(s)
	var maxLen int
	var maxSubStr string

	for i := range runes {
		length, str := spread(runes, i, i, maxLen) // aba
		if length > maxLen {
			maxLen = length
			maxSubStr = str
		}
		length, str = spread(runes, i, i+1, maxLen) // abba
		if length > maxLen {
			maxLen = length
			maxSubStr = str
		}
	}

	return maxSubStr
}

// 以中心点向两侧扩张，获取当前字符为中心的最大回文串
func spread(runes []rune, l, r int, curMaxLen int) (length int, subStr string) {
	length = curMaxLen
	for ; l >= 0 && r <= len(runes)-1 && runes[l] == runes[r]; l, r = l-1, r+1 {
		if r-l+1 > length {
			length = r - l + 1
			subStr = string(runes[l : r+1])
		}
	}
	return
}
```

执行耗时：**48 ms (43%)**，优化了取子串判断回文的逻辑，遍历与扩张双层循环复杂度为 `O(N^2)`



### O(N) Manacher 算法

参考：[Longest Palindromic Substring Part II](https://articles.leetcode.com/longest-palindromic-substring-part-ii/)

在 `O(N^2)` 的解法中，奇偶长度的回文串因为出发点不同，需要分类讨论。`O(N)`

- 马拉车算法判断子串回文的扩张出发点永远只有一个：

  算法对整个字符串做了预处理：在字符间插入一个不曾出现的分割符，如 `#`，使得所有回文子串长度均变为了奇数：`aba` => `#a#b#a#`，`abba` => `#a#b#b#a#`

- 引入辅助数组 `radius` 来标识每个字符的回文半径：

  ```go
  t      = # a # b # a
  radius = 0 1 0 3 0 1
  
  t      = # a # b # b # a #
  radius = 0 1 0 1 4 1 0 1 0
  ```

  最大 `radius` 就对应着最长回文子串。

算法也使用向左向右扩张的方式来算最长回文子串的半径，但高效的原因是 **没有做重复的回文判断**：

<div class="tip">
大回文子串内的字符，回文半径初始值不是 1，而是 min( 对称点回文半径，距离右边缘距离 )    

如果回文能突破右边界，则该字符成为新的最长回文中心
</div>

结合代码与配图理解：

![image-20181123133321959](https://images.yinzige.com/2018-11-23-053322.png)

```go
func longestPalindrome(s string) string {
	if len(s) <= 1 {
		return s
	}

	t := "#"
	for _, r := range s {
		t += string(r) + "#" // 统一将 偶数串/奇数串 -> 奇数串
	}

	radius := make([]int, len(t)) // 每个位置的回文半径
	maxCenter := 0                // 到目前为止最长回文串的中心位置
	maxR := 0                     // 到目前位置最长回文串的右边界索引
	stopCenter := 0
	maxRadius := 0

	for i := 0; i < len(t); i++ {
		mirror := 2*maxCenter - i
		if i < maxR { // 没有超过最大右边界
			radius[i] = min(radius[mirror], maxR-i) // 取 [i,mx] 与 i'半径 的最小值
		}

		// 以 i 为中心不断扩充半径，向左右两侧探测回文
		// 不要以为这里复杂度为 O(N)
		// 若上边 radius[i] = radius[i']，则不会再进入循环扩张半径
		for i+1+radius[i] < len(t) && i-1-radius[i] >= 0 && t[i-1-radius[i]] == t[i+1+radius[i]] {
			radius[i]++
		}

		// 超出了边缘
		if i+radius[i] > maxR {
			maxR = i + radius[i]
			maxCenter = i
		}

		if radius[i] > maxRadius {
			maxRadius = radius[i]
			stopCenter = i
		}
	}
	l := stopCenter/2 - maxRadius/2 // t->s，索引取中
	r := l + maxRadius - 1          // 减掉自身
	return string(s[l : r+1])       // bingo
}

func min(x, y int) int {
	if x < y {
		return x
	}
	return y
}
```

执行耗时：**8 ms (80%)**



## 最佳实现

Manacher 算法。



## 总结

字符串的特征与匹配有很多精妙的算法，如 `KMP`，AC 自动机，Manacher 等算法，温故而知新吧。