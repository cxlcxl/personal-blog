---
title: "Vue el-tree 的 default-checked-keys 属性默认选中状态bug问题"
date: 2022-11-21T15:31:53+08:00
draft: false
description: "el-tree 的 default-checked-keys 属性默认选中状态bug问题"
categories: ["vue"]
tags: ["vue", "element"]
ShowToc: true
ShowBreadCrumbs: '' #原创文章，如需转载请注明文章作者和出处。谢谢！
---

解决方式：
> 无需设置 `default-checked-keys` 属性
1. 读取到接口数据后使用 `this.$refs.tree.setCheckedNodes(***传入接口返回的节点 keys***)` 设置默认；
2. 提交前使用 `this.$refs.tree.getCheckedKeys()` 再次获取选中的节点并赋值