---
title: "Go 打乱数据排序"
date: 2022-11-29T14:39:24+08:00
draft: false
description: "打乱数据排序"
categories: ["golang"]
ShowToc: true
ShowBreadCrumbs: ''
---

```go
// func shuffle[T any](slice []T) { // go1.18 及以上
func shuffle(slice []int64) {
  if len(slice) == 1 {
    return
  }
  r := rand.New(rand.NewSource(time.Now().UnixNano()))
  for len(slice) > 0 {
    n := len(slice)
    randIndex := r.Intn(n)
    slice[n-1], slice[randIndex] = slice[randIndex], slice[n-1]
    slice = slice[:n-1]
  }
}
```