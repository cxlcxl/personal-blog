---
title: "Vue3 Vite 使用基础配置"
date: 2022-11-22T17:53:38+08:00
draft: false
description: "Vite 使用基础配置"
categories: ["vue"]
tags: ["vue3", "vite"]
ShowToc: false
ShowBreadCrumbs: '' #原创文章，如需转载请注明文章作者和出处。谢谢！
---

```javascript
import { defineConfig, loadEnv } from "vite"
import vue from "@vitejs/plugin-vue"
import path from "path"
import { nodePolyfills } from "vite-plugin-node-polyfills"

// https://vitejs.dev/config/
export default defineConfig(({ command, mode }) => {
  console.log("vite.config defineConfig", command, mode)
  const env = loadEnv(mode, process.cwd(), "")
  return {
    plugins: [
      vue(),
      nodePolyfills({
        protocolImports: true,
      }),
    ],
    resolve: {
      alias: {
        "@": path.resolve(__dirname, "src"),
        "@v": path.resolve(__dirname, "src/views"),
        "@a": path.resolve(__dirname, "src/apis"),
      },
    },
    define: {
      "process.env": env,
    },
    server: {
      proxy: {
        "/api/": {
          target: process.env.VUE_APP_BASE_API,
          changeOrigin: true,
          rewrite: (path) => path.replace(/^\/api/, ""),
        },
      },
    },
  }
})

```