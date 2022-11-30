---
title: "最长无重复子串"
date: 2022-11-30T18:02:12+08:00
draft: false
description: "最长无重复子串"
categories: ["algorithm"]
showToc: true
---

### 无重复字符的最长子串
> 给定一个字符串 s ，请你找出其中不含有重复字符的 最长子串 的长度。

#### 示例 1
- 输入: s = "abcabcbb"
- 输出: 3 
- 解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。

#### 示例 2
- 输入: s = "bbbbb"
- 输出: 1
- 解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。

#### 示例 3
- 输入: s = "pwwkew"
- 输出: 3
- 解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。

### 解题
#### 1. 粗暴阶梯法
解题思路：`abcabcbb`
指针遍历，从 0 开始，遇到重复字符指针加 1，重新从 1 开始遍历，直到结束

```go
func lengthOfLongestSubstring(s string) int {
    if s == "" {
        return 0
    }
    tmp := make(map[byte]struct{})
    l, idx, maxLen := 0, 0, 0
    bs := []byte(s)
    for idx < len(s) {
        if _, ok := tmp[bs[idx]]; ok {
            if idx-l > maxLen {
                maxLen = idx - l
            }
            l++
            idx = l
            tmp = map[byte]struct{}{bs[idx]: {}}
        } else {
            tmp[bs[idx]] = struct{}{}
        }
        idx++
    }
    if idx-l > maxLen {
        maxLen = idx - l
    }
    return maxLen
}
```