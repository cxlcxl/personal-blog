---
author: "Silent"
title: "K8s - Kubeadm 部署安装"
date: 2022-11-20T10:25:27+08:00
lastMod: 2022-11-20T10:25:27+08:00
draft: false
description: "k8s Kubeadm 部署安装"
categories: ["k8s"]
tags: ["k8s"]
ShowToc: true
ShowBreadCrumbs: '' #原创文章，如需转载请注明文章作者和出处。谢谢！
---

## kube-proxy开启ipvs的前置条件 

```shel
modprobe br_netfilter

cat > /etc/sysconfig/modules/ipvs.modules <<EOF
#!/bin/bash
modprobe -- ip_vs
modprobe -- ip_vs_rr
modprobe -- ip_vs_wrr
modprobe -- ip_vs_sh
modprobe -- nf_conntrack_ipv4
EOF

chmod 755 /etc/sysconfig/modules/ipvs.modules && bash /etc/sysconfig/modules/ipvs.modules && lsmod | grep -e ip_vs -e nf_conntrack_ipv4
```
  > 如果报错：`modprobe: FATAL: Module nf_conntrack_ipv4 not found.`
  > 内核高版本的 `nf_conntrack_ipv4` 已被 `nf_conntrack` 替换，将以上的 `nf_conntrack_ipv4` 改为 `nf_conntrack` 重新执行。


## 安装 Docker 软件

```shell
yum install -y yum-utils device-mapper-persistent-data lvm2 # docker 依赖包

# docker 镜像源
yum-config-manager \
  --add-repo \
  http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
 
yum install -y docker-ce

## 创建 /etc/docker 目录
mkdir /etc/docker

# 配置 daemon.
cat > /etc/docker/daemon.json <<EOF
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "insecure-registries": ["harbor.hongfu.com"],
  "registry-mirrors": [
    "--阿里云镜像加速地址--"
  ]
}
EOF
mkdir -p /etc/systemd/system/docker.service.d

# 重启docker服务
systemctl daemon-reload && systemctl restart docker && systemctl enable docker
```

## 安装 Kubeadm （主从配置）

```shel
cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=http://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=0
repo_gpgcheck=0
gpgkey=http://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg http://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
EOF

yum -y  install  kubeadm-1.15.1 kubectl-1.15.1 kubelet-1.15.1
systemctl enable kubelet.service
```

导入 k8s 镜像，有梯子可以直接下载：`kubeadm config images pull`
> v1.15.1 版本的镜像网盘下载地址如下：
> 链接: https://pan.baidu.com/s/1aAHyZXVjHr0Wclh6xFJV2w 提取码: b269

```bash
# 8s.gcr.io/kube-apiserver
# 8s.gcr.io/kube-proxy
# 8s.gcr.io/kube-scheduler
# 8s.gcr.io/kube-controller-manager
# 8s.gcr.io/etcd
# 8s.gcr.io/pause
# 8s.gcr.io/coredns/coredns

docker save [-o | >] 保存的镜像名.tar [镜像名:tag | 镜像ID]   # 如果以镜像ID导出，导入时需要自己重新修改镜像名称和 tag
docker load [-i | <] 保存的镜像名.tar
docker tag 镜像ID 镜像名:镜像tag
```

## 初始化主节点

```she
kubeadm config print init-defaults > kubeadm-config.yaml # 打印一个初始化配置到 kubeadm-config.yaml 文件

# 调整刚刚打印出来的 kubeadm-config.yaml 文件修改点如下：
localAPIEndpoint:
  advertiseAddress: 192.168.66.10 # 本机IP
  kubernetesVersion: v1.15.1 
networking:
  podSubnet: "10.244.0.0/16" # flannel 要求网段
  serviceSubnet: 10.96.0.0/12
---
apiVersion: kubeproxy.config.k8s.io/v1alpha1
kind: KubeProxyConfiguration
featureGates:
  SupportIPVSProxyMode: true
mode: ipvs

kubeadm init --config=kubeadm-config.yaml --experimental-upload-certs | tee kubeadm-init.log
```
> 安装日志
```bash
Flag --experimental-upload-certs has been deprecated, use --upload-certs instead
W0719 15:03:54.748649    7060 strict.go:54] error unmarshaling configuration schema.GroupVersionKind{Group:"kubeadm.k8s.io", Version:"v1beta2", Kind:"InitConfiguration"}: error unmarshaling JSON: while decoding JSON: json: unknown field "kubernetesVersion"
[init] Using Kubernetes version: v1.15.1
[preflight] Running pre-flight checks
	[WARNING IsDockerSystemdCheck]: detected "cgroupfs" as the Docker cgroup driver. The recommended driver is "systemd". Please follow the guide at https://kubernetes.io/docs/setup/cri/
	[WARNING SystemVerification]: this Docker version is not on the list of validated versions: 20.10.10. Latest validated version: 18.09
[preflight] Pulling images required for setting up a Kubernetes cluster
[preflight] This might take a minute or two, depending on the speed of your internet connection
[preflight] You can also perform this action in beforehand using 'kubeadm config images pull'
[kubelet-start] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
[kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[kubelet-start] Activating the kubelet service
[certs] Using certificateDir folder "/etc/kubernetes/pki"
[certs] Generating "front-proxy-ca" certificate and key
[certs] Generating "front-proxy-client" certificate and key
[certs] Generating "etcd/ca" certificate and key
[certs] Generating "etcd/server" certificate and key
[certs] etcd/server serving cert is signed for DNS names [master-1 localhost] and IPs [192.168.0.101 127.0.0.1 ::1]
[certs] Generating "etcd/peer" certificate and key
[certs] etcd/peer serving cert is signed for DNS names [master-1 localhost] and IPs [192.168.0.101 127.0.0.1 ::1]
[certs] Generating "etcd/healthcheck-client" certificate and key
[certs] Generating "apiserver-etcd-client" certificate and key
[certs] Generating "ca" certificate and key
[certs] Generating "apiserver" certificate and key
[certs] apiserver serving cert is signed for DNS names [master-1 kubernetes kubernetes.default kubernetes.default.svc kubernetes.default.svc.cluster.local] and IPs [10.96.0.1 192.168.0.101]
[certs] Generating "apiserver-kubelet-client" certificate and key
[certs] Generating "sa" key and public key
[kubeconfig] Using kubeconfig folder "/etc/kubernetes"
[kubeconfig] Writing "admin.conf" kubeconfig file
[kubeconfig] Writing "kubelet.conf" kubeconfig file
[kubeconfig] Writing "controller-manager.conf" kubeconfig file
[kubeconfig] Writing "scheduler.conf" kubeconfig file
[control-plane] Using manifest folder "/etc/kubernetes/manifests"
[control-plane] Creating static Pod manifest for "kube-apiserver"
[control-plane] Creating static Pod manifest for "kube-controller-manager"
[control-plane] Creating static Pod manifest for "kube-scheduler"
[etcd] Creating static Pod manifest for local etcd in "/etc/kubernetes/manifests"
[wait-control-plane] Waiting for the kubelet to boot up the control plane as static Pods from directory "/etc/kubernetes/manifests". This can take up to 4m0s
[kubelet-check] Initial timeout of 40s passed.
[apiclient] All control plane components are healthy after 40.503426 seconds
[upload-config] Storing the configuration used in ConfigMap "kubeadm-config" in the "kube-system" Namespace
[kubelet] Creating a ConfigMap "kubelet-config-1.15" in namespace kube-system with the configuration for the kubelets in the cluster
[upload-certs] Storing the certificates in Secret "kubeadm-certs" in the "kube-system" Namespace
[upload-certs] Using certificate key:
df949aae79dfa71766351e9ca59deaf1dfcee045d46121d037881d42ddcbcf06
[mark-control-plane] Marking the node master-1 as control-plane by adding the label "node-role.kubernetes.io/master=''"
[mark-control-plane] Marking the node master-1 as control-plane by adding the taints [node-role.kubernetes.io/master:NoSchedule]
[bootstrap-token] Using token: abcdef.0123456789abcdef
[bootstrap-token] Configuring bootstrap tokens, cluster-info ConfigMap, RBAC Roles
[bootstrap-token] configured RBAC rules to allow Node Bootstrap tokens to post CSRs in order for nodes to get long term certificate credentials
[bootstrap-token] configured RBAC rules to allow the csrapprover controller automatically approve CSRs from a Node Bootstrap Token
[bootstrap-token] configured RBAC rules to allow certificate rotation for all node client certificates in the cluster
[bootstrap-token] Creating the "cluster-info" ConfigMap in the "kube-public" namespace
[addons] Applied essential addon: CoreDNS
[addons] Applied essential addon: kube-proxy

Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 192.168.0.101:6443 --token abcdef.0123456789abcdef \
    --discovery-token-ca-cert-hash sha256:39a62e1abd02ce75c82b39cb81798446266316fbc16ebd286049116184c82287
```

  > 如果报错：`unknown flag: --experimental-upload-certs`
  > 可能是版本升级的原因，将以上的 `--experimental-upload-certs` 改为 `--upload-certs` 重新执行。

## 加入主节点以及其余工作节点

```shell
执行安装日志中的加入命令即可
```

## 使用 flannel 部署 k8s 的扁平化网络

> 首先需要下载 flannel 的 docker 镜像：`quay.io/coreos/flannel:v0.12.0-amd64`
> flannel 镜像与配置文件地址如下：
> 链接: https://pan.baidu.com/s/13g41uMWuQEu-CSpfhWmTTA 提取码: a2nn
```shell
# -f 后为配置文件，如已下载可直接指向下载了的文件
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```


> 附加：已启动的 master 查寻 token 和 hash 值加入
```bash
# 查询 token
kubeadm token list
# 查询 hash 值
openssl x509 -pubkey -in /etc/kubernetes/pki/ca.crt | openssl rsa -pubin -outform der 2>/dev/null | openssl dgst -sha256 -hex | sed 's/^.* //'
```