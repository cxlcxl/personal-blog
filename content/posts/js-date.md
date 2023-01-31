---
title: "Js 日期处理"
date: 2023-01-31T10:56:23+08:00
draft: false
description: ""
categories: [""]
tags: [""]
ShowToc: true
ShowBreadCrumbs: ''
cover:
  image: ""
---

```js
// 7 天前的日期「3600 * 1000 * 24 * 7 几天前都可以以此方式得到」
const start = new Date()
start.setTime(start.getTime() - 3600 * 1000 * 24 * 7)

// 本月第一天
const start = new Date(new Date().setDate(1))
// 由上 -1 天 可以取到上月最后一天
start.setTime(start.getTime() - 3600 * 1000 * 24 * 1)

// 延伸得到上月日期
const end = new Date(new Date().setDate(1)) 
end.setTime(end.getTime() - 3600 * 1000 * 24 * 1)
const s = new Date(end - 0) // 日期深拷贝
const start = new Date(s.setDate(1))
// 得到 start, end 上月的第一天和最后一天
```