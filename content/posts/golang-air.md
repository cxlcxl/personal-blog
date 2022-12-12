---
author: "Silent"
title: "golang air 热更新使用"
date: 2022-11-21T11:25:27+08:00
lastMod: 2022-11-21T11:25:27+08:00
draft: false
description: "air 热更新使用"
categories: ["golang"]
tags: ["golang"]
ShowToc: true
ShowBreadCrumbs: "" #原创文章，如需转载请注明文章作者和出处。谢谢！
---

1. 首先安装 air 运行命令：`go install github.com/cosmtrek/air@latest`
2. 项目根目录创建 air 配置文件：`.air.conf`

> mac 系统文件内容

```toml
# .air.conf 文件内容
root = "."
tmp_dir = "tmp"

[build]
# Just plain old shell command. You could use `make` as well.
cmd = "go build -o ./tmp/main ./web/main.go"
# Binary file yields from `cmd`.
bin = "tmp/main"
# Customize binary.
full_bin = "APP_ENV=dev APP_USER=air ./tmp/main"
# Watch these filename extensions.
include_ext = ["go", "tpl", "tmpl", "html"]
# Ignore these filename extensions or directories.
exclude_dir = ["storage", "web/storage"]
# Watch these directories if you specified.
include_dir = []
# Exclude files.
exclude_file = []
# This log file places in your tmp_dir.
log = "air.log"
# It's not necessary to trigger build each time file changes if it's too frequent.
delay = 1000 # ms
# Stop running old binary when build errors occur.
stop_on_error = true
# Send Interrupt signal before killing process (windows does not support this feature)
send_interrupt = false
# Delay after sending Interrupt signal
kill_delay = 500 # ms

[log]
# Show log time
time = false

[color]
# Customize each part's color. If no color found, use the raw app log.
main = "magenta"
watcher = "cyan"
build = "yellow"
runner = "green"

[misc]
# Delete tmp directory on exit
clean_on_exit = true
```

> windows 系统文件内容

```toml
# 工作目录
# 使用 . 或绝对路径，请注意 `tmp_dir` 目录必须在 `root` 目录下
root = "."
tmp_dir = "tmp"

[build]
# 只需要写你平常编译使用的shell命令。你也可以使用 `make`
cmd = "go build -o ./tmp/main.exe ./public/main.go"
# 由`cmd`命令得到的二进制文件名
bin = "tmp/main.exe"
# 自定义的二进制，可以添加额外的编译标识例如添加 GIN_MODE=release
# full_bin = "SET APP_ENV=dev & SET APP_USER=air & ./tmp/main.exe"
full_bin = "./tmp/main.exe"
# 监听以下文件扩展名的文件.
include_ext = ["go", "tpl", "tmpl", "html"]
# 忽略这些文件扩展名或目录
exclude_dir = ["storage", "public/storage", "tmp"]
# 监听以下指定目录的文件
include_dir = []
# 排除以下文件
exclude_file = []
# 如果文件更改过于频繁，则没有必要在每次更改时都触发构建。可以设置触发构建的延迟时间
delay = 1000 # ms
# 发生构建错误时，停止运行旧的二进制文件。
stop_on_error = true
# air的日志文件名，该日志文件放置在你的`tmp_dir`中
log = "air_errors.log"

[log]
# 显示日志时间
time = true

[color]
# 自定义每个部分显示的颜色。如果找不到颜色，使用原始的应用程序日志。
main = "magenta"
watcher = "cyan"
build = "yellow"
runner = "green"

[misc]
# 退出时删除tmp目录
clean_on_exit = true
```

3. 运行命令：`air` 或 `air -d`
