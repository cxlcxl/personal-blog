---
title: "Git 忽略文件使用"
date: 2022-11-24T11:11:31+08:00
draft: false
description: "git 忽略文件使用"
categories: ["git"]
tags: ["git忽略文件", "git"]
ShowToc: true
ShowBreadCrumbs: '' #原创文章，如需转载请注明文章作者和出处。谢谢！
---
----

### 匹配中间文件夹

示例 `.gitignore` 内容：匹配 `app` 下的所有模块文件夹下 `config` 下以 `.pro.yaml` 结尾的文件
```txt
// app/user/config/databse.pro.yaml
// app/goods/config/databse.pro.yaml
// ...
app/**/config/*.pro.yaml
```

### 忽略已被提交到远程仓库的文件

1. 先移除本地 `git` 缓存
```git
git rm --cached [要忽略的文件路径]
```

2. 补充 `.gitignore` 文件内容即可
