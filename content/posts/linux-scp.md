---
title: "Linux scp 文件管理"
date: 2022-11-21T16:31:51+08:00
draft: false
description: "scp 文件管理"
categories: ["linux"]
tags: ["scp"]
ShowToc: true
ShowBreadCrumbs: '' #原创文章，如需转载请注明文章作者和出处。谢谢！
---

```shell
# 从服务器上下载文件
scp [服务器用户名]@[服务器IP]:[服务器文件] [本地目录]

# 上传本地文件到服务器
scp [本地文件] [服务器用户名]@[服务器IP]:[服务器目录]

# 从服务器下载整个目录
scp -r [服务器用户名]@[服务器IP]:[服务器目录] [本地目录]

# 上传目录到服务器
scp -r [本地目录] [服务器用户名]@[服务器IP]:[服务器目录]
```