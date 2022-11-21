---
author: "Silent"
title: "K8s - 资源清单"
date: 2022-11-21T10:25:27+08:00
lastMod: 2022-11-21T10:25:27+08:00
draft: false
description: "k8s 资源清单"
categories: ["k8s"]
tags: ["k8s"]
ShowToc: true
ShowBreadCrumbs: '' #原创文章，如需转载请注明文章作者和出处。谢谢！
---

## 资源清单格式

```yam
apiVersion: group/apiversion  # 如果没有给定 group 名称，那么默认为 core，可以使用 kubectl api-versions # 获取当前 k8s 版本上所有的 apiVersion 版本信息( 每个版本可能不同 )
kind:       #资源类别
metadata：  #资源元数据
   name
   namespace
   lables
   annotations   # 主要目的是方便用户阅读查找
spec: # 期望的状态（disired state）
status：# 当前状态，本字段有 Kubernetes 自身维护，用户不能去定义
```

<!--配置清单主要有五个一级字段，其中status用户不能定义，有k8s自身维护-->

## 资源清单的常用命令

**获取 apiversion 版本信息**

```shell
[root@k8s-master01 ~]# kubectl api-versions 
admissionregistration.k8s.io/v1beta1
apiextensions.k8s.io/v1beta1
apiregistration.k8s.io/v1
apiregistration.k8s.io/v1beta1
apps/v1
......(以下省略)
```


**获取资源的 apiVersion 版本信息**

```shel
[root@k8s-master01 ~]# kubectl explain pod
KIND:     Pod
VERSION:  v1
.....(以下省略)

[root@k8s-master01 ~]# kubectl explain Ingress
KIND:     Ingress
VERSION:  extensions/v1beta1
```



**获取字段设置帮助文档**

```shell
[root@k8s-master01 ~]# kubectl explain pod
KIND:     Pod
VERSION:  v1

DESCRIPTION:
     Pod is a collection of containers that can run on a host. This resource is
     created by clients and scheduled onto hosts.

FIELDS:
   apiVersion    <string>
     ........
     ........
```

**字段配置格式**

```yaml
apiVersion <string>          #表示字符串类型
metadata <Object>            #表示需要嵌套多层字段
labels <map[string]string>   #表示由k:v组成的映射
finalizers <[]string>        #表示字串列表
ownerReferences <[]Object>   #表示对象列表
hostPID <boolean>            #布尔类型
priority <integer>           #整型
name <string> -required-     #如果类型后面接 -required-，表示为必填字段
```



## 通过定义清单文件创建 Pod

```yaml
apiVersion: v1 # 根绝对象类型可以查询 kubectl explain pod 取 version 的值
kind: Pod  # 对象类型先确定
metadata:
  name: pod-demo
  namespace: default
  labels:
    app: myapp
spec:
  containers:
  - name: myapp-1
    image: harbor.hongfu.com/library/myapp:v1
  - name: busybox-1
    image: harbor.hongfu.com/library/busybox:v1
    command: # 替换镜像默认封装的命令
    - "/bin/sh"
    - "-c"
    - "sleep 3600"
```

```she
kubectl get pod xx.xx.xx -o yaml  
<!--使用 -o 参数 加 yaml，可以将资源的配置以 yaml的格式输出出来，也可以使用json，输出为json格式--> 
```

<!--资源被创建的流程：Kubectl（Yaml） >>> KubeapiServer（Json）-->

```base
kubectl create -f [yaml-filename] # 创建资源
kubectl explain pod # 获取 kind 所属的 apiVersion
    # 读取子对象配置
    kubectl explain pod.metadata
kubectl get pod
    -n kube-system [pod-name] -o yaml # 查看某个 pod yaml 配置清单 -n 名称控件，默认 default
    -o wide
kubectl exec -it [pod-name] -c [container-name] # 进入某个 pod 的容器
kubectl logs [pod-name]
kubectl describe pod [pod-name]
kubectl delete [pod | svc] [--all | pod-name] # 删除 pod、service
```