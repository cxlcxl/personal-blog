---
title: "从远程仓库定时自动发版"
date: 2022-11-22T14:15:52+08:00
draft: false
description: "从远程仓库定时自动发版"
categories: ["linux", "git"]
tags: ["自动发版", "git", "shell"]
ShowToc: true
ShowBreadCrumbs: '' #原创文章，如需转载请注明文章作者和出处。谢谢！
---

1. 添加脚本文件到你期望存放的地址

```bash
#!/bin/bash
SITEDIR=/www/projects/site
BAKDIR=/www/baks
DAYS=7
DATE=`date +%Y%m%d`
REMOTE=`gitee/github/gitlab 仓库地址`

# -c  压缩
# -x  解压
# -z  支持gzip解压文件
# -v  显示操作过程
# -f  使用档名，请留意，在f之后要立即接档名！不要再加参数！
tar -zcvf $BAKDIR/site-$DATE-bak.tar.gz $SITEDIR
cd /www/projects/
rm -rf site

git clone $REMOTE site

# 删除过期备份文件
# $bakdir find 备份文件的地址
#-type    f    类型为普通文件
#-mtime   7    天之前的文件
#-exec rm -f   静默删除匹配出来的文件
# 还可以 -name "site-*-.tar.gz" 名称模糊匹配
find $BAKDIR -type f -mtime +$DAYS -exec rm -f {} \;

echo "complete!"
```

2. 运行 `crontab -e` 设置定时任务运行上一步添加的文件

