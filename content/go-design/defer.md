---
title: "Go 语言设计与实现阅读笔记：Defer"
date: 2023-01-10T15:32:46+08:00
draft: false
description: "Go 语言设计与实现阅读：Defer"
categories: [""]
tags: [""]
ShowToc: true
ShowBreadCrumbs: ''
cover:
  image: ""
---

原文地址：<a href="https://draveness.me/golang/docs/part2-foundation/ch05-keyword/golang-defer/" target="__blank">defer</a>

#### 什么是 `defer`
- `defer`：在函数返回前，会执行 `defer` 关键字所传入的函数。
#### 提出问题
1. 多个 `defer` 的运行顺序是什么
2. 传值时 `defer` 执行结果是否符合预期

#### `defer` 原理分析
.....

#### 问题解释
- 后调用的 `defer` 函数会先执行：
  - 后调用的 `defer` 函数会被追加到 Goroutine `_defer` 链表的最前面；
  - 运行 runtime._defer 时是从前到后依次执行；
- 函数的参数会被预先计算；
  - 调用 runtime.deferproc 函数创建新的延迟调用时就会立刻拷贝函数的参数，函数的参数不会等到真正执行时计算；
