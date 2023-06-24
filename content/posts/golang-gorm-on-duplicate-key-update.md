---
title: "Golang Gorm 插入数据遇到重复时修改"
date: 2023-01-04T19:26:22+08:00
draft: false
description: "Golang Gorm on Duplicate Key Update"
categories: ["golang", "gorm"]
tags: [""]
ShowToc: true
ShowBreadCrumbs: ''
cover:
  image: ""
---


- `golang` `gorm` 代码如下
```golang
func (m *User) BatchInsert(users []*User) (err error) {
  if len(users) == 0 {
    return nil
  }
  // 要修改的字段
  updateColumns := []string{"email"}
  return m.Table(m.TableName()).Clauses(clause.OnConflict{
    DoUpdates: clause.AssignmentColumns(updateColumns),
  }).CreateInBatches(users, 500).Error
}

```

- `debug` 生成的代码，当数据重复时，会修改 `email` 字段，而不是插入
```sql
INSERT INTO `users`(...) VALUES(...) ON DUPLICATE KEY UPDATE `email`=VALUES(`email`)
```