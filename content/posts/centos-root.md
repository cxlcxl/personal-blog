---
title: "因 /dev/mapper/centos-root 空间不足而导致 docker 等服务运行失败"
date: 2023-03-06T16:13:48+08:00
draft: true
description: ""
categories: [""]
tags: [""]
ShowToc: true
ShowBreadCrumbs: ''
cover:
  image: ""
---

```shell
[root@k8sm1 /]# df -h
文件系统                 容量  已用  可用 已用% 挂载点
devtmpfs                 1.4G     0  1.4G    0% /dev
tmpfs                    1.4G     0  1.4G    0% /dev/shm
tmpfs                    1.4G  588K  1.4G    1% /run
tmpfs                    1.4G     0  1.4G    0% /sys/fs/cgroup
/dev/mapper/centos-root   13G  8.1G  4.9G   63% /
/dev/sda1               1014M  227M  788M   23% /boot
k8sm1                    466G  137G  329G   30% /www
tmpfs                    280M     0  280M    0% /run/user/0
```