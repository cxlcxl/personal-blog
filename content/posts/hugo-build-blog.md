---
title: "使用 Hugo 搭建自己的博客"
date: 2022-11-24T20:48:28+08:00
draft: false
description: "使用 Hugo 搭建自己的博客"
categories: ["hugo"]
tags: ["hugo", "博客搭建", "搭建博客"]
ShowToc: true
ShowBreadCrumbs: ''
cover:
  image: ""
---

### Hugo 的安装

- 安装可查看 <a href="https://gohugo.io/installation/" target="_blank">官方文档</a>
- 安装完成后按照文档的快速启动项目操作
```bash
# 创建一个项目
hugo new site [项目名称，如：myblog]

# 进入你的项目，后面所有的命令都是在项目根目录执行（未特殊标明的）
cd myblog

# 初始化 git 仓库
git init
git remote add origin [你的 github/gitee/gitlab 仓库地址]
```

### 下载一个你喜欢的主题
- 官方文档 <a href="https://themes.gohugo.io/" target="_blank">主题库</a> 这里有很多
- 我使用的是这个 <a href="https://github.com/adityatelange/hugo-PaperMod" target="_blank">主题</a> stars 还挺多的
- 直接在你的项目目录执行下面 `git` 命令就可以了
```git
git clone [主题仓库地址] themes/主题名
```

### 配置你的主题
- 项目根目录 `config.toml` 文件
```toml
# 设置主题
theme = "PaperMod"
```
- 具体主题相关配置可按照自己下载的主体来，如我使用的这个主体可以在他的仓库看下有 `exampleSite` 分支，切到这个分支下可找到 `config.yml` 文件

> PS：如果主题的配置文件是 `yml` 或 `yaml` 格式，可以随便百度或 google `yml to toml` 解决配置转换问题

### 创建你的内容

- 运行如下命令
```bash
hugo new posts/first-post.md
```
- 在 `content/posts/` 里可以看到刚刚创建的 md 文件，一般内容头部（更多可以在官网查看）

```txt
title: "使用 Hugo 搭建自己的博客"
date: 2022-11-24T20:48:28+08:00
draft: false                            # 是否是草稿，构建的时候可以用上
description: "使用 Hugo 搭建自己的博客"
categories: ["hugo"]                    # 数组，分类可以用上
tags: ["hugo", "博客搭建"]               # 数组，标签可以用上
ShowToc: true                          # 是否显示内容页的目录
cover:
  image: ""                            # 列表展示的图片
```

### 启动

```bash
hugo server -D # -D buildDraft 构建草稿


Start building sites …
hugo v0.106.0+extended darwin/amd64 BuildDate=unknown
....
....
Environment: "development"
Serving pages from memory
Running in Fast Render Mode. For full rebuilds on change: hugo server --disableFastRender
Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)
Press Ctrl+C to stop
```

- 这时就可以在浏览器打开你构建出来的地址了：`http://localhost:1313/`

### 发布

- 直接运行 `hugo` 命令，完成后可以在根目录找到 `public` 目录，是纯 `html` 格式的文件，将整个目录丢到 `nginx` 或其他服务器运行就可以了
```bash
hugo
```
> 附上 `nginx` 的 `https` 配置，如果不需要 `https` 直接去掉注释标注 `https` 中间部分然后监听 80 端口即可
```conf
server {
    #SSL 默认访问端口号为 443
    #listen 80;
    listen 443 ssl;
    #请填写绑定证书的域名
    server_name www.silent-cxl.top silent-cxl.top;

    ############# https #############
    #请填写证书文件的相对路径或绝对路
    ssl_certificate cert/silent-cxl.top.crt;
    #请填写私钥文件的相对路径或绝对路径
    ssl_certificate_key cert/silent-cxl.top.key;
    ssl_session_timeout 5m;
    #请按照以下协议配置
    ssl_protocols TLSv1.2 TLSv1.3;
    #请按照以下套件配置，配置加密套件，写法遵循 openssl 标准。
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE;
    ssl_prefer_server_ciphers on;
    ############# https #############

    root /usr/share/nginx/html/site/public/;
    index index.html index.htm;

    access_log  /var/log/nginx/access.hugosite.log main;
    error_log   /var/log/nginx/error.hugosite.log;

    location / {
        try_files $uri $uri/ /index.html;
    }

    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }
}

############# https #############
server {
  listen 80;
  #请填写绑定证书的域名
  server_name www.silent-cxl.top;
  #把http的域名请求转成https
  return 301 https://$host$request_uri;
}
############# https #############
```