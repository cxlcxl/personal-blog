---
title: "Git Pull 时报错：拒绝合并无关的历史"
date: 2023-01-05T17:02:46+08:00
draft: false
description: "Git Pull 时报错：拒绝合并无关的历史"
categories: ["git"]
tags: ["git"]
ShowToc: true
ShowBreadCrumbs: ''
cover:
  image: ""
---

- `git` 在拉代码时报一下错误
```git
➜  scripts git:(master) git pull origin master
来自 .......[远程仓库地址]
 * branch            master     -> FETCH_HEAD
致命错误：拒绝合并无关的历史
```

- 原因是仓库与本地版本不一致导致的，但确认代码是一致的，直接运行以下 `git` 命令
```git
git pull origin master --allow-unrelated-histories
```