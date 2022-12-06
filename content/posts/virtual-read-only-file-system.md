---
title: "VirtualBox 虚拟机 Read-only file system"
date: 2022-12-06T10:56:41+08:00
draft: false
description: "VirtualBox 虚拟机 Read-only file system"
categories: ["虚拟机"]
tags: ["虚拟机","deploy"]
ShowToc: true
ShowBreadCrumbs: ''
---

**在虚拟机中创建软连接时报错 Read-only file system，修改方法如下**

1. 关闭虚拟机
2. 用管理员身份启用cmd，进入到虚拟机安装目录(C:\Program Files\Oracle\VirtualBox)
3. 运行以下命令

```shell
VBoxManage setextradata YOURVMNAME VBoxInternal2/SharedFoldersEnableSymlinksCreate/YOURSHAREFOLDERNAME 1
```
- `YOURVMNAME` ：虚拟机中系统名称
- `YOURSHAREFOLDERNAME` ：虚拟机本地共享文件夹名称(不用带路径)

查看是否配置成功

```shell
VBoxManage getextradata YOURVMNAME enumerate
```

用管理员身份开启虚拟机，OK！

参考博文  
<a href="https://blog.csdn.net/qazx123q/article/details/88894482" target="_blank">https://blog.csdn.net/qazx123q/article/details/88894482</a>
<a href="https://ahtik.com/fixing-your-virtualbox-shared-folder-symlink-error/" target="_blank">https://ahtik.com/fixing-your-virtualbox-shared-folder-symlink-error/</a>
