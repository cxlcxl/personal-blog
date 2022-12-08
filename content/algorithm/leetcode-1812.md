---
title: "Leetcode:1812、判断国际象棋棋盘中一个格子的颜色"
date: 2022-12-08T14:50:36+08:00
draft: false
description: ""
categories: ["algorithm"]
showToc: false
---

- 给你一个坐标 coordinates ，它是一个字符串，表示国际象棋棋盘中一个格子的坐标。下图是国际象棋棋盘示意图。
<div align="center"><img src="/images/chessboard.png" width="350"/></div>
- 如果所给格子的颜色是白色，请你返回 true，如果是黑色，请返回 false 。
- 给定坐标一定代表国际象棋棋盘上一个存在的格子。坐标第一个字符是字母，第二个字符是数字。

#### 示例 1：
```txt
输入：coordinates = "a1"
输出：false
解释：如上图棋盘所示，"a1" 坐标的格子是黑色的，所以返回 false 。
```


> `go` 题解
```golang
func squareIsWhite(coordinates string) bool {
  // a-h 的字母转换 byte 为 ascii 数字，转换单双，一个单一个双才是白色
  return (coordinates[0]%2)^(coordinates[1]%2) == 1
}
```