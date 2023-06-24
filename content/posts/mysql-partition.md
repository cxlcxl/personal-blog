---
title: "Mysql 设置数据库分区"
date: 2023-02-06T13:52:34+08:00
draft: false
description: "Mysql 设置数据库分区"
categories: ["mysql"]
tags: ["mysql"]
ShowToc: true
ShowBreadCrumbs: ''
cover:
  image: ""
---

```sql
-- 前面 4 步是操作分区的字段（设置为主键 PRIMARY），如果要分区的字段已是主键可跳过这几步

-- 1. 移除 id 的自增
alter table [tablename] modify id bigint(20)unsigned;

-- 2. 移除主键
alter table [tablename] drop PRIMARY KEY;

-- 3. 重新添加主键
alter table [tablename] add PRIMARY KEY(id,stat_day);

-- 4. 重新设置 id 自增
alter table [tablename] modify id bigint(20)unsigned AUTO_INCREMENT;

-- 5. 正式分区语句
ALTER TABLE [tablename] PARTITION BY RANGE (TO_DAYS(stat_day)) (
    PARTITION p20231 VALUES LESS THAN (TO_DAYS('2023-01-01')), -- TO_DAYS('20230101')
    PARTITION p20232 VALUES LESS THAN (TO_DAYS('2023-02-01')),
    PARTITION p20233 VALUES LESS THAN (TO_DAYS('2023-03-01'))
);

-- 查询数据，检查是否只查询了数据所在的分区
explain partitions select * from report_ads_sources where stat_day = '2023-01-06'

-- 6 删除一个分区(分区的所有数据也都会被删除)
alter table [tablename] drop PARTITION p20231;

-- 7 新增一个分区
alter table [tablename] add PARTITION (PARTITION p20234 VALUES LESS THAN (TO_DAYS('2023-04-01')));

-- 8 新增一个分区(不满足其余分区条件的都存放在这个分区)
alter table [tablename] add PARTITION (PARTITION p20235 VALUES LESS THAN maxvalue);
