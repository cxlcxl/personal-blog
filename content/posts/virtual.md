---
title: "Virtual 虚拟机安装 Centos7"
date: 2023-02-23T17:53:24+08:00
draft: false
description: ""
categories: ["虚拟机"]
tags: [""]
ShowToc: true
ShowBreadCrumbs: ''
cover:
  image: ""
---

### 为什么要使用虚拟机来搭建开发环境

1. 对于新手即可以熟悉 Linux 使用，同时也可以减少或避免在 windows 系统上安装服务时出现的奇葩问题
2. 还可以与测试正式服务器统一运行环境，减少开发时出现的“在我这都不会”问题
3. 反正有很多好处就行了

### 软件准备
> <a href="https://www.virtualbox.org/wiki/Downloads" target="__blank">VirtualBox</a> 本人版本 6.1
需要 6.1 版本的可以点此下载：
<div align="center">
 <img src="/images/virtualbox6.1.jpg" />
</div>

> <a href="https://mirrors.aliyun.com/centos/7/isos/x86_64/" target="__blank">Centos</a> 本人版本 7.9「CentOS-7-x86_64-Minimal-2207-02.iso」