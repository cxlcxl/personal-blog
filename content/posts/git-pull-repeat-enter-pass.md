---
title: "Git 解决每次 git pull 需要输入账号密码"
date: 2022-11-21T15:23:32+08:00
draft: false
description: "解决每次 git pull 需要输入账号密码"
categories: ["git"]
ShowToc: true
ShowBreadCrumbs: '' #原创文章，如需转载请注明文章作者和出处。谢谢！
---

git bash 进入项目目录：
```git
git config --global credential.helper store
```
输入以上命令后，重新拉取代码再次输入账号密码，输入后会记住，以后拉代码不需要再输入。
