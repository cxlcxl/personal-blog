---
title: "Virtual 虚拟机扩容（/dev/mapper/centos-root 空间不足）"
date: 2022-11-21T15:26:19+08:00
draft: false
description: "虚拟机扩容（/dev/mapper/centos-root 空间不足）"
categories: ["虚拟机"]
tags: ["虚拟机"]
ShowToc: true
ShowBreadCrumbs: '' #原创文章，如需转载请注明文章作者和出处。谢谢！
---

#### 查看根分区大小
 > df -h

```bash
文件系统                类型      容量  已用  可用 已用% 挂载点
/dev/mapper/centos-root xfs        18G  1.1G   17G    6% /
devtmpfs                devtmpfs  479M     0  479M    0% /dev
tmpfs                   tmpfs     489M     0  489M    0% /dev/shm
tmpfs                   tmpfs     489M  6.7M  483M    2% /run
tmpfs                   tmpfs     489M     0  489M    0% /sys/fs/cgroup
/dev/sda1               xfs       497M  125M  373M   25% /boot
tmpfs                   tmpfs      98M     0   98M    0% /run/user/0
```
#### 在虚拟机中添加一块物理的磁盘，重起虚拟机
#### 查看磁盘编号
> ls /dev/sd*
```bash
/dev/sda  /dev/sda1  /dev/sda2  /dev/sdb
```
#### 创建pv
> pvcreate /dev/sdb
```bash
Physical volume "/dev/sdb" successfully created
```
#### 把 pv 加入 vg 中，相当于扩充 vg 的大小
- 先使用 vgs 查看 vg 组

> vgs
```bash
VG #PV #LV #SN Attr VSize VFree
centos 2 2 0 wz--n- 59.50g 20.04g
```
 
- 扩展 vg，使用 vgextend 命令
> vgextend centos /dev/sdb

#### 把 vg 卷扩展了，在用 vgs 查看一下
> vgs
```bash
VG     #PV #LV #SN Attr   VSize  VFree
centos   2   2   0 wz--n- 39.50g 20.04g
```
> lvs
```bash
LV   VG     Attr       LSize  Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  root centos -wi-ao---- 17.47g                                                   
  swap centos -wi-ao----  2.00g  虽然我们把vg扩展了，但是lv还没有扩展
 ```
#### 扩展 lv，使用 lvextend 命令
> lvextend -L +20G /dev/mapper/centos-root
```bash
Size of logical volume centos/root changed from 17.47 GiB (4472 extents) to 37.47 GiB (9592 extents).
Logical volume root successfully resized
 ```
#### 命令使系统重新读取大小
> xfs_growfs /dev/mapper/centos-root
```bash
meta-data=/dev/mapper/centos-root isize=256    agcount=4, agsize=1144832 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=0        finobt=0
data     =                       bsize=4096   blocks=4579328, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0 ftype=0
log      =internal               bsize=4096   blocks=2560, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
data blocks changed from 4579328 to 9822208
 ```
#### 再使用 df  -h 查看
> df -h
```bash
文件系统                 容量  已用  可用 已用% 挂载点
/dev/mapper/centos-root   38G  1.1G   37G    3% /
devtmpfs                 479M     0  479M    0% /dev
tmpfs                    489M     0  489M    0% /dev/shm
tmpfs                    489M  6.7M  483M    2% /run
tmpfs                    489M     0  489M    0% /sys/fs/cgroup
/dev/sda1                497M  125M  373M   25% /boot
tmpfs                     98M     0   98M    0% /run/user/0
```

转载自：<a href="https://www.cnblogs.com/feiyun126/p/7680534.html" target="_blank">虚拟机扩容（/dev/mapper/centos-root 空间不足</a>