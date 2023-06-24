---
title: "Mysql 修改数据库、表字符集"
date: 2023-01-30T14:57:01+08:00
draft: false
description: "Mysql 修改数据库、表字符集"
categories: ["mysql"]
tags: [""]
ShowToc: true
ShowBreadCrumbs: ''
cover:
  image: ""
---

- 修改数据库字符集
```sql
ALTER DATABASE hiads DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;
```
- 修改表字符集
```sql
ALTER TABLE jobs CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;
```
- 修改字段的字符集
```sql
ALTER TABLE oauth_access_tokens CHANGE scopes scopes VARCHAR(128) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;
```