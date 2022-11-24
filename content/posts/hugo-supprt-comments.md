---
title: "Hugo PaperMod 开启评论"
date: 2022-11-24T20:29:38+08:00
draft: false
description: "Hugo PaperMod 开启评论"
categories: ["hugo"]
tags: ["hugo", "PaperMod", "开启评论"]
ShowToc: true
ShowBreadCrumbs: ''
---

### 创建评论使用的 github 仓库

- 操作步骤如下图
![](/images/hugo-utteranc-comment-step.png)

- 创建好后如我的：`https://github.com/cxlcxl/comments-for-hugo`，而后可在最底下的 `Enable Utterances` 部分取到配置

### Hugo 配置文件文件修改
`config.toml`
```toml
[params]
# ...
comments = true
  #使用的是 utteranc 评论，教程参考 https://utteranc.es/
  [params.utteranc]
  enable = true
  repo = "cxlcxl/comments-for-hugo" # 改成自己的配置
  issueTerm = "pathname"
  theme = "github-light"
```

### comments.html 文件修改

文件位置：`themes/PaperMod/layouts/partials/comments.html`
```html
{{- /* Comments area start */ -}}
{{- /* to add comments read => https://gohugo.io/content-management/comments/ */ -}}

{{ if .Site.Params.utteranc.enable }}
<div class="post bg-white">
  <script src="https://utteranc.es/client.js"
      repo= "{{ .Site.Params.utteranc.repo }}"
      issue-term="{{ .Site.Params.utteranc.issueTerm }}"
      theme="{{ .Site.Params.utteranc.theme }}"
      crossorigin="anonymous"
      async>
  </script>
</div>
{{ end }}

{{- /* Comments area end */ -}}
```