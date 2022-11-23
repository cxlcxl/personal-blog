---
title: "Vue3 页面登录拦截器"
date: 2022-11-22T17:26:30+08:00
draft: false
description: "页面登录拦截器"
categories: ["vue"]
tags: ["vue3"]
ShowToc: true
ShowBreadCrumbs: '' #原创文章，如需转载请注明文章作者和出处。谢谢！
---

> 直接上代码


```javascript
// intercept.js
import NProgress from "nprogress"
import "nprogress/nprogress.css" // progress bar style
import store from "@/store"
import router from "./router"
import settings from "./settings"
import { getToken } from "@/utils/token"
import { ElMessage } from "element-plus"

NProgress.configure({ showSpinner: false })
const excludeUris = ["/login", "/sso/callback"]

router.beforeEach(async (to, from, next) => {
  NProgress.start()
  // 顺便设置 icon title
  document.title = to.meta.title
    ? `${to.meta.title} - ${settings.title}`
    : settings.title

  const token = getToken()
  if (token) {
    if (to.path === "/login") {
      NProgress.done()
      next({ path: "/" })
    } else {
      if (store.getters.user_id && store.getters.user_id > 0) {
        next()
      } else {
        const { user_id } = await store.dispatch("user/getUserInfo")
        if (user_id > 0) {
          next({ ...to, replace: true })
        } else {
          ElMessage.error({ message: "个人信息读取失败" })
          NProgress.done()
          next(`/login?redirect=${to.path}`)
        }
      }
    }
  } else {
    NProgress.done()
    if (excludeUris.includes(to.path)) {
      next()
    } else {
      next({ path: "/login" })
    }
  }
})

router.afterEach(() => {
  NProgress.done()
})
```

```javascript
// main.js
import "./intercept"
```
