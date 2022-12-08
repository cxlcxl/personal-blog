---
title: "Leetcode:1775、通过最少操作次数使数组的和相等"
date: 2022-12-08T14:31:21+08:00
draft: false
description: ""
categories: ["algorithm"]
showToc: false
---

- 给你两个长度可能不等的整数数组 nums1 和 nums2 。两个数组中的所有值都在 1 到 6 之间（包含 1 和 6）。
- 每次操作中，你可以选择 任意 数组中的任意一个整数，将它变成 1 到 6 之间 任意 的值（包含 1 和 6）。
- 请你返回使 nums1 中所有数的和与 nums2 中所有数的和相等的最少操作次数。如果无法使两个数组的和相等，请返回 -1 。

#### 示例 1
```txt
输入：nums1 = [1,2,3,4,5,6], nums2 = [1,1,2,2,2,2]
输出：3
解释：你可以通过 3 次操作使 nums1 中所有数的和与 nums2 中所有数的和相等。以下数组下标都从 0 开始。
- 将 nums2[0] 变为 6 。 nums1 = [1,2,3,4,5,6], nums2 = [6,1,2,2,2,2] 。
- 将 nums1[5] 变为 1 。 nums1 = [1,2,3,4,5,1], nums2 = [6,1,2,2,2,2] 。
- 将 nums1[2] 变为 2 。 nums1 = [1,2,2,4,5,1], nums2 = [6,1,2,2,2,2] 。
```

#### 示例 2
```txt
输入：nums1 = [1,1,1,1,1,1,1], nums2 = [6]
输出：-1
解释：没有办法减少 nums1 的和或者增加 nums2 的和使二者相等。
```

> `go` 题解
```golang
func minOperations(nums1 []int, nums2 []int) int {
  if len(nums1) > 6*len(nums2) || len(nums2) > 6*len(nums1) {
    return -1
  }
  // 先算出两个数组相差多少
  n := 0
  for _, v := range nums1 { n += v }
  for _, v := range nums2 { n -= v }
  var smallNums, largeNums []int
  if n > 0 {
    smallNums, largeNums = nums2, nums1
  } else {
    n *= -1
    smallNums, largeNums = nums1, nums2
  }
  // 排序方便以最大的差值变动数组元素
  sort.Ints(smallNums)
  sort.Ints(largeNums)
  k := 0
  s, l := 0, len(largeNums)-1
  for n > 0 {
    isSmall := true
    diff := 0
    // 小的数组以最大值 6 扣减算出差额
    if s < len(smallNums) && smallNums[s] < 6 {
      diff = 6 - smallNums[s]
    }
    // 大的数组以最小值 1 扣减算出差额
    if l >= 0 && largeNums[l] > 1 {
      if largeNums[l]-1 > diff { // diff 哪个差额大用哪个数组的元素变动
        diff = largeNums[l] - 1 
        isSmall = false
      }
    }
    // 扣减并叠加步数
    n -= diff
    k++
    if n <= 0 {
      break
    }
    if isSmall {
      s++
    } else {
      l--
    }
  }
  
  return k
}

```