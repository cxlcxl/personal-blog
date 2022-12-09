---
title: "记录 Github Actions 实现代码提交自动部署"
date: 2022-11-30T11:04:47+08:00
draft: false
description: "使用 github actions 实现代码提交自动部署"
categories: ["hugo"]
tags: ["github", "actions", "deploy"]
ShowToc: true
ShowBreadCrumbs: ''
---

### 先上 `github-acitons` 部署 `yml` 文件内容

```yaml
name: 部署到服务器
on:
  push:
    branches: [ "master" ] # master 分支有提交自动部署

permissions: write-all
jobs:
  deploy:
    name: Deploy
    runs-on: ubuntu-latest
    environment: production

    steps:
    # 获取源码 master 分支，也可以写 tag 版本
    - name: Checkout
      uses: actions/checkout@master

    # 使用到的这个 easingthemes/ssh-deploy actions，main 表示主分支
    # 可以直接仓库搜索到：easingthemes/ssh-deploy，有介绍下面的配置项
    - name: 部署到服务器
      uses: easingthemes/ssh-deploy@main
      env:
        SSH_PRIVATE_KEY: ${{ secrets.BLOG_ECS_KEY }}
        ARGS: "-avzr --delete"  # 部署前删除目录
        SOURCE: "public/"
        REMOTE_HOST: ${{ secrets.BLOG_ECS_HOST }} 
        REMOTE_USER: ${{ secrets.BLOG_ECS_USER }}
        TARGET: ${{ secrets.BLOG_ECS_TARGET }}
```

### 首先开启 `github` 仓库的 `actions` 功能

> 两种方式
#### 第一种
1. 进入 `github` 找到你要部署的仓库
2. 进入 `actions` 后点击 -> `Skip this and **set up a workflow yourself**`
3. 编写 `yml` 文件
#### 第二种
直接在项目根目录创建 `.github/workflows/deploy.yml` 随项目一起提交（文件名可自定义）

### `easingthemes/ssh-deploy` 配置

- `SSH_PRIVATE_KEY`

  > 按以下步骤执行
  1. 进入你的服务器，在 `/root/.ssh/` 目录查看是否有 `id_rsa.pub` 公钥和 `id_rsa` 私钥
  2. 没有：运行 `ssh-keygen -m PEM -t rsa -b 4096` 生成，有就跳过此步
  3. `cp id_rsa.pub authorized_keys`
  4. `cat id_rsa` 查看并复制私钥，要复制全部内容
  5. 此时回到仓库，将第4步复制的私钥填入仓库的 `settings.Secrets.Actions` 按照定义的 `key.value` 添加

- `SOURCE`

  要部署的项目目录，如 `dist`,`public`

- `REMOTE_HOST`

  服务器 IP

- `REMOTE_USER`

  服务器用户名

- `TARGET`

  要部署到服务器哪个目录