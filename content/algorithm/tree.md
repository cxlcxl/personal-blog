---
title: "递归数"
date: 2022-11-30T14:12:36+08:00
draft: false
description: "递归数"
categories: ["algorithm"]
showToc: false
---

&emsp;&emsp;在计算机科学中，树（英语：tree）是一种抽象数据类型（ADT）或是实现这种抽象数据类型的数据结构，用来模拟具有树状结构性质的数据集合。它是由n（n>0）个有限节点组成一个具有层次关系的集合。把它叫做“树”是因为它看起来像一棵倒挂的树，也就是说它是根朝上，而叶朝下的。它具有以下的特点：

- 每个节点都只有有限个子节点或无子节点；
- 没有父节点的节点称为根节点；
- 一个非根节点有且只有一个父节点；
- 了根节点外，每个子节点可以分为多个不相交的子树；
- 里面没有环路(cycle)

<div align="center">
 <img src="/images/algorithm-tree.png" width="300" height="290" alt="图片名称"/>
</div>

> `go` 语言递归树代码
```go
package main

import (
  "fmt"
)

type tree struct {
  id   int
  pid  int
  node string
  sub  []tree
}

func main() {
  t := []tree{
    {1, 0, "a", nil},
    {2, 0, "b", nil},
    {3, 1, "a1", nil},
    {4, 1, "a2", nil},
    {5, 1, "a3", nil},
    {6, 3, "a11", nil},
    {7, 3, "a12", nil},
    {8, 4, "a21", nil},
    {9, 2, "b1", nil},
    {10, 2, "b2", nil},
    {11, 10, "b21", nil},
  }
  fmt.Println(formatTree(t, 0))
}

func formatTree(t []tree, pid int) (res []tree) {
  for _, v := range t {
    if pid == v.pid {
      v.sub = formatTree(t, v.id)
      res = append(res, v)
    }
  }
  return
}
```