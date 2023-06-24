---
title: "Centos 安装 docker"
date: 2023-02-23T18:26:48+08:00
draft: false
description: ""
categories: ["docker", "centos"]
tags: [""]
ShowToc: true
ShowBreadCrumbs: ''
cover:
  image: ""
---

### Centos 安装 Docker
本机通过 Virtualbox 安装 Centos 虚拟机（需自行安装）后，安装 Docker：
如果安装使用的非 root 账号，需要加 sudo
1. 卸载旧版本，如未安装过旧版本可不执行
```shell
yum remove docker \
    docker-client \
    docker-client-latest \
    docker-common \
    docker-latest \
    docker-latest-logrotate \
    docker-logrotate \
    docker-selinux \
    docker-engine-selinux \
    docker-engine
```
2. 安装依赖包
```shell
yum install -y yum-utils
```
3. 添加 yum 软件源（不建议使用官方 Docker 安装源，下载很慢）
```shell
yum-config-manager \
    --add-repo \
    https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
    
    
sed -i 's/download.docker.com/mirrors.aliyun.com\/docker-ce/g' /etc/yum.repos.d/docker-ce.repo
```
4. 安装 Docker 引擎
```shell
yum install docker-ce docker-ce-cli containerd.io
```
5. 启动 Docker 并将 Docker 设置为开机自启
```shell
systemctl start docker

systemctl enable docker

docker version
```
6. 另外通过 docker-compose 搭建本地环境，需再另外安装 docker-compose 插件
```shell
curl -L https://download.fastgit.org/docker/compose/releases/download/1.27.4/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose

docker-compose version
```
7. 配置 docker 镜像加速，文件地址 `/etc/docker/daemon.json`
```json
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m", "max-file": "10"
  },
  "insecure-registries": ["harbor.hongfu.com"],
  "registry-mirrors": [
    "可使用阿里云镜像加速地址"
  ]
}
```
```shell
systemctl daemon-reload && systemctl restart docker
```
阿里云镜像加速地址获取方式：
> 登陆阿里云控制台 -> 搜索 “容器镜像服务” -> 镜像工具 -> 镜像加速器