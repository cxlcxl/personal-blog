---
title: "Vue3 使用 Vuex 做登录信息"
date: 2022-11-22T17:26:30+08:00
draft: false
description: "使用 Vuex 做登录信息"
categories: ["vue"]
tags: ["vue3", "vuex"]
ShowToc: true
ShowBreadCrumbs: '' #原创文章，如需转载请注明文章作者和出处。谢谢！
---

**&emsp;&emsp;Vuex 是一个专为 Vue.js 应用程序开发的状态管理模式 + 库。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。**

----
目录结构：
```text
|--public
|--src
...
|----store # 新建 vuex 相关文件夹
|------modules # 新建模块文件夹，存放 app.js,user.js
|--------global.js
|--------getters.js
|--------app.js
|--------user.js
```

#### `store` 文件夹下创建 `index.js`
```javascript
import { createStore } from "vuex"

import global from "./modules/global.js"
import app from "./modules/app.js"   // 应用基本数据
import user from "./modules/user.js" // 登录信息
import getters from "./modules/getters.js"

export default createStore({
  // 公共模板直接在这里展开就可以
  ...global,
  modules: {
    app,
    user,
  },
  getters,
})
```

#### `main.js` 中导入
```javascript
import store from "./store/index.js"

createApp(App).use(store)

// ...
```

#### `modules` 文件夹下创建 `app.js`,`user.js`
```javascript
// user.js 内容，app.js 与 user.js 结构一致
import md5 from "js-md5"
import { login, getUserInfo, logout } from "@a/user"
import { setToken, getToken, delToken } from "@/utils/token"

// 用户
export default {
  namespaced: true,
  state: {
    email: "",
    username: "",
    token: "",
    user_id: "",
  },
  mutations: {
    setLoginInfo(state, { email, user_id, username }) {
      state.email = email
      state.user_id = user_id
      state.username = username
    },
    setToken(state, { token }) {
      state.token = token
      setToken(token)
    },
  },
  actions: {
    login(store, { email, pass }) {
      return new Promise((resolve, reject) => {
        login({ email, pass: md5(pass) })
          .then((res) => {
            const { email, token, user_id, username } = res.data
            store.commit("setLoginInfo", { email, user_id, username })
            store.commit("setToken", { token })
            resolve(res.data)
          })
          .catch(() => {
            reject()
          })
      })
    },
    getUserInfo(store) {
      return new Promise((resolve, reject) => {
        getUserInfo(getToken())
          .then((res) => {
            const { email, user_id, username } = res.data
            store.commit("setLoginInfo", { email, user_id, username })
            resolve(res.data)
          })
          .catch(() => {
            delToken()
            reject()
          })
      })
    },
    logout(store) {
      return new Promise((resolve, reject) => {
        logout()
          .then(() => {
            delToken()
            resolve()
          })
          .catch(() => {
            reject()
          })
      })
    },
  },
}


// getters.js 内容
export default {
  user_id: (state, getters) => state.user.user_id,
  email: (state, getters) => state.user.email,
  username: (state, getters) => state.user.username,
  token: (state, getters) => state.user.token,
}

// global.js 内容
export default {
  state: {},
  mutations: {},
  actions: {},
  getters: {},
}
```

#### vue3 页面组件中使用方式
```javascript
import { useStore } from "vuex"

const store = useStore() // store 就是 vuex 对象

// 例
store.dispatch("user/login", form).then((res) => {
  // 成功的逻辑
  // ...
}).catch(() => {})
```
