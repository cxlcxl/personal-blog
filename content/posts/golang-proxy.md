---
title: "Golang 环境搭建"
date: 2022-11-21T17:54:32+08:00
draft: false
description: "go 环境搭建"
categories: ["golang"]
ShowToc: true
ShowBreadCrumbs: '' #原创文章，如需转载请注明文章作者和出处。谢谢！
---

#### 安装运行时遇到过的问题
- 问题：遇到 `go install` 报错：`.... dial tcp 172.217.160.113:443: i/o timeout` 
  - 解决，终端执行：`go env -w GOPROXY=https://goproxy.cn`

- `mac` 系统运行时：`listen tcp :18081: bind: address already in use`
  - `lsof -ti:18081`
  - `kill [port]`

待续...