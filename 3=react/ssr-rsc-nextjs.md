# 服务端渲染与 RSC (Server Components)

## 1. SSR (Server Side Rendering)
在服务器生成 HTML，减少首屏白屏时间。

## 2. RSC (React Server Components)
- **概念**：仅在服务器运行的组件，不向浏览器发送 JS。
- **优势**：减小包体积，直接访问数据库/文件系统。
- **区分**：`'use client'` 表示客户端组件，`'use server'` 表示服务端操作。

## 3. 水合 (Hydration)
服务器生成 HTML 后，浏览器加载 JS 并将事件监听器绑定到 DOM 上的过程。

## 4. Next.js App Router
App Router 默认所有组件均为 Server Components，开启了 React 的全异步数据流获取模式。
