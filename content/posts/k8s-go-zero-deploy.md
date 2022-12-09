---
title: "K8s 上 Go-Zero 服务本机手动部署记录"
date: 2022-11-24T15:42:05+08:00
draft: false
description: "K8s 上 Go-Zero 服务本机手动部署记录"
categories: ["k8s", "go-zero"]
tags: ["k8s", "go-zero", "deployment"]
ShowToc: true
ShowBreadCrumbs: '' #原创文章，如需转载请注明文章作者和出处。谢谢！
---

### 创建命名空间
- 如果在`default`命名空间上可以跳过此步骤
```bash
kubectl create ns bs

// 可以查看刚刚创建的 namespace
kubectl get ns
```

### 创建 k8s 账号权限
- 创建 `ads-test-auth.yaml` 配置文件如下
```yaml
#创建账号
apiVersion: v1
kind: ServiceAccount
metadata:
  namespace: bs
  name: ads-test

---
#创建角色对应操作
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: discovery-ads-test
rules:
- apiGroups: [""]
  resources: ["endpoints"] # endpoints
  verbs: ["get","list","watch"]

---
#给账号绑定角色
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: find-ads-test-discovery-ads-test
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: discovery-ads-test
subjects:
- kind: ServiceAccount
  name: ads-test
  namespace: bs
```
运行 `kubectl apply -f ads-test-auth.yaml` 创建资源

### 进入你的 go-zero 项目

> #### API 模块

1. 如果原配置文件的 `rpc` 使用的是：`Endpoints`，可以改成：`Target` 方式，示例：

```yaml
# ....
UserRpcClient:
  # 原
  #Endpoints:
  #  - "127.0.0.1:8002"
  
  # 改为 结构 => k8s://namespace/service:RPC 端口
  Target: "k8s://bs/user-rpc-svc:8002"
# ....
```

2. 生成 `dockerfile`
```bash
goctl docker -go user.go
```

3. 切换到项目根路径制作镜像

```bash
docker build -f xxxxxxx/Dockerfile -t xx:xx .
```
  - 可以推送到远程或本地镜像仓库方便多节点部署，本机也可以直接 `docker save` 之后上传

4. 进入服务目录生成 api K8S yaml 文件
```bash
goctl kube --nodePort 30010 deploy -replicas 3 -requestCpu 100 -requestMem 50 -limitCpu 200 -limitMem 100 -name user-api -namespace bs -image user-api:v1.0 -o user-api.yaml -port 8001 --serviceAccount find-endpoints
```
- —nodePort K8S 限制端口在 30000-? 之间，不填则系统随机

> #### RPC 模块
1. rpc docker 文件创建与 api 一致
2. 进入 rpc 服务目录生成 K8S yaml 文件
```bash
goctl kube deploy -replicas 3 -requestCpu 100 -requestMem 50 -limitCpu 200 -limitMem 100 -name user-rpc -namespace bs -image user-rpc:v1.0 -o user-rpc.yaml -port 8002 --serviceAccount find-endpoints
```

### 运行生成的 k8s 文件
- 如果多节点且 docker 镜像没有上传镜像仓库，需要提前上传
```bash
kubectl apply -f user-rpc.yaml
kubectl apply -f user-api.yaml

# 检查问题与查看资源清单命令
kubectl get pod,svc,deploy -n bs # 一起查看，单个查看分开执行
kubectl describe pod [pod名] -n bs # 可以查看执行详细情况
kubectl logs [pod名] -n bs # 查看日志
```

> 等待运行完成本机就可以通过 api 的 yaml 文件定义的端口 30010 进行访问了