---
title: "Docker Compose Port Is Already Allocated"
date: 2022-12-06T10:54:37+08:00
draft: false
description: "docker-compose up 报错：Bind for 0.0.0.0:8098 failed: port is already allocated"
categories: ["docker-compose"]
tags: ["docker-compose", "docker", "deploy"]
ShowToc: true
ShowBreadCrumbs: ''
---

```bash
ERROR: for local_nginx  Cannot start service nginx: driver failed programming external connectivity on endpoint local_nginx (c3d788a28999dc7888524cd93d63a5dac3da0fe00d9c7d8a09e8bb815005b9f0): Bind for 0.0.0.0:8098 failed: port is already allocated
```
容器没有运行了，但是端口还在绑定占用

解决：
1. `systemctl stop docker.service`
2. `rm /var/lib/docker/network/files/local-kv.db`
3. `systemctl restart docker.service`