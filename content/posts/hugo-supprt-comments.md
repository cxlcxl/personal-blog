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

- 操作步骤如下图（utteranc 的操作截图）
- <a href="https://giscus.app/zh-CN" target="_blank">giscus</a> 驱动与 <a href="https://utteranc.es/" target="_blank">utteranc</a> 驱动开启操作大同小异，步骤基本相同

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
  enable = false
  repo = "cxlcxl/comments-for-hugo" # 改成自己的配置
  issueTerm = "pathname"
  theme = "github-light"

  ## 配置 giscus 评论,教程参考 https://giscus.app/zh-CN
  [params.giscus]
  enable = true
  repo = "cxlcxl/comments-for-hugo" # 改成自己的配置
  repoId = ""
  category = ""
  categoryId = ""
  mapping = "pathname"
  theme = "light_protanopia"
  lang = "zh-CN"
  crossorigin = "anonymous"
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

{{ else if .Site.Params.giscus.enable }}

<div class="post bg-white">
  <script src="https://giscus.app/client.js"
        data-repo="{{ .Site.Params.giscus.repo }}"
        data-repo-id="{{ .Site.Params.giscus.repoId }}"
        data-category="{{ .Site.Params.giscus.category }}"
        data-category-id="{{ .Site.Params.giscus.categoryId }}"
        data-mapping="{{ .Site.Params.giscus.mapping }}"
        data-strict="0"
        data-reactions-enabled="1"
        data-emit-metadata="0"
        data-input-position="bottom"
        data-theme="{{ .Site.Params.giscus.theme }}"
        data-lang="{{ .Site.Params.giscus.lang }}"
        crossorigin="{{ .Site.Params.giscus.crossorigin }}"
        async>
</script>
</div>

{{ end }}

{{- /* Comments area end */ -}}

```