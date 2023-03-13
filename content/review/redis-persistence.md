---
title: "Redis 数据持久化"
date: 2023-02-28T11:50:21+08:00
draft: true
description: ""
categories: ["redis"]
tags: ["redis"]
ShowToc: true
ShowBreadCrumbs: ''
cover:
  image: ""
---

&emsp;&emsp;Redis 是内存数据库，数据一般存储在内存中，但是为了数据安全性「进程退出时内存中的数据丢失」，所以需要将内存中的数据持久化到磁盘中，出现进程退出再重启 Redis 服务时可以从磁盘中重新加载持久化的数据。

### Redis 的两种持久化机制
- `RDB`「数据保存磁盘」
- `AOF`「操作指令保存磁盘」


#### RDB
Redis 默认开启了 RDB 机制持久化数据，持久化策略在配置文件中可以找到，触发任何一个条件将会自动持久化，如下：
```conf
save 900 1    # 900s 内修改了 1 次
save 300 10   # 300s 内修改了 10 次
save 60 10000 # 60s 内修改了 10000 次
```

- `RDB` 自动持久化机制：Fork 一个子进程来创建新的 `RDB` 文件，记录接收到 `BGSAVE` 当时的数据库状态，父进程继续处理接收到的命令，子进程完成文件的创建之后，会发送信号给父进程，父进程处理命令的同时，通过轮询来接收子进程的信号
- `RDB` 还可以通过 `SAVE` 指令来持久化「阻塞 `Redis` 进程，直到 `RDB` 文件创建完毕」


#### AOF
`AOF` 持久化是通过将所有写状态的指令以 `append` 的形式保存到 `AOF` 的持久化文件中
```conf
appendonly yes
appendfilename "appendonly.aof"
# 三种方式：
# no       等待操作系统处理
# always   每次更新都持久化
# everysec 每秒调用一次
appendfsync everysec
# aof重写期间是否同步
no-appendfsync-on-rewrite no
# 重写触发配置
auto-aof-rewrite-percentage 100
auto-aof-rewrite-min-size 64mb
```
#### RDB / AOF 比较
