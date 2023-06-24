---
title: "Virtual 本地虚拟机网络配置与共享文件夹"
date: 2022-11-21T15:34:03+08:00
draft: false
description: "本地虚拟机网络配置与共享文件夹"
categories: ["虚拟机"]
tags: ["虚拟机"]
ShowToc: true
ShowBreadCrumbs: '' #原创文章，如需转载请注明文章作者和出处。谢谢！
---

#### 网络
> 虚拟机版本：`CentOS Linux release 7.9.2009 (Core)`

配置文件地址：`/etc/sysconfig/network-scripts/ifcfg-enp0s3`
```bash
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=static
DEFROUTE=yes
IPADDR=192.168.120.61
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=enp0s3
UUID=03dae35b-d264-45e4-8d73-8920ab61cd02
DEVICE=enp0s3
ONBOOT=yes
GETRWAY=192.168.56.1
NETMASK=255.255.255.0
DNS1=192.168.56.1
```

#### 共享文件夹

配置文件地址：`/etc/rc.local` 或 `/etc/rc.d/rc.local` 在最后加一行
```bash
mount -t vboxsf [你 virtualbox 上绑定的共享文件夹名] /www
```
