---
title: "Css 文字色彩动画"
date: 2022-12-05T14:26:59+08:00
draft: false
description: "Css 文字色彩动画"
categories: ["css"]
tags: ["css", "文字色彩动画"]
ShowToc: true
ShowBreadCrumbs: ''
---

> <div class="css-font-color-animation">记录一个遇到的文字色彩动画</div>

```css
.example {
  display: block;
  text-decoration: none;
  background-image: -webkit-linear-gradient(
    left,
    #3498db,
    #f47920 10%,
    #d71345 20%,
    #f7acbc 30%,
    #ffd400 40%,
    #3498db 50%,
    #f47920 60%,
    #d71345 70%,
    #f7acbc 80%,
    #ffd400 90%,
    #3498db
  );
  color: transparent;
  -webkit-background-clip: text;
  background-size: 200% 100%;
  animation: slide 5s infinite linear;
  font-size: 40px;
}

@keyframes slide {
  0% {
    background-position: 0 0;
  }
  100% {
    background-position: -100% 0;
  }
}
```

> 原地址：<a href="http://github.xyxiao.cn/vue-cropper/example/" target="_blank">图片裁剪</a>