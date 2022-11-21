---
author: "Silent"
title: "go-zero goctl 使用 proto 引入其他 proto 文件"
date: 2022-11-21T10:25:27+08:00
lastMod: 2022-11-21T10:25:27+08:00
draft: false
description: "goctl 使用 proto 引入其他 proto 文件"
categories: ["go-zero"]
tags: ["proto", "go-zero"]
ShowToc: true
ShowBreadCrumbs: '' #原创文章，如需转载请注明文章作者和出处。谢谢！
---

## 编写 proto 文件
```proto
// role.proto 文件
syntax = "proto3";

option go_package = "./marketing";

package marketing;

message RoleCreateReq {
  string role_name = 1;
}
```

```proto
// user.proto 文件
syntax = "proto3";

option go_package = "./rbac";

package rbac;
import "pb/role.proto"; // 以执行 goctl 命令为起始路径

message UserCreateReq {
  string username = 1;
  string email = 2;
}

message BaseResp {
  int64 code = 1;
}

service RbacCenter{
  // RPC 服务
  rpc RoleCreate(RoleCreateReq) returns(BaseResp);

  rpc UserCreate(UserCreateReq) returns(BaseResp);
}
```

> 两个文件放在同一目录内

## 执行 goctl 与 原始 protoc 命令
```bash
goctl rpc protoc ./pb/user.proto --go_out=. --go-grpc_out=. --zrpc_out=. -style goZero # 生成代码

protoc ./pb/role.proto --go_out=. --go-grpc_out=.  # 生成 pb.go 数据结构文件
```
> 进入 rbaccenter 目录映射一下 type 数据类型