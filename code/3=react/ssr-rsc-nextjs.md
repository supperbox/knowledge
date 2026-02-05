# 服务端渲染 (SSR) 与 Server Components (RSC)

React 的边界正在向服务端扩展，核心逻辑在于：**让服务器做它擅长的事（存取数据），让客户端做它擅长的事（交互）**。

---

## 1. SSR (Server Side Rendering)

- **原理**：在服务器上把所有组件跑一遍，生成完整的 HTML 字符串发给浏览器。
- **优点**：SEO 友好，首屏渲染快。
- **缺点**：浏览器拿到 HTML 后，依然要下载全量 JS 并执行一遍（水合 Hydration），才能变的可交互。

---

## 2. RSC (React Server Components)

这是 React 18 引入的革命性架构，目前主要在 Next.js App Router 中大规模使用。

### 2.1 什么是 RSC？

RSC 只在服务器端执行，它的渲染结果（不是 HTML，是一种特殊的流数据）被发送到浏览器。
**RSC 的代码永远不会被下载到浏览器中**，这极大地减小了首屏 JS 包体积。

### 2.2 核心区别

| 特性         | 服务端组件 (Server)           | 客户端组件 (Client) |
| :----------- | :---------------------------- | :------------------ |
| 指令         | 默认即是                      | `'use client'`      |
| 访问数据库   | ✅ 可以直接 `await db.find()` | ❌ 必须通过 API     |
| 交互 / Hooks | ❌ 无 `onClick`, `state`      | ✅ 完整 React 能力  |
| 包体积影响   | 🚀 零体积                     | 📦 正常打包         |

---

## 3. 水合 (Hydration) 与岛屿架构

- **水合**：将静态 HTML “激活”的过程。
- **痛点**：全量水合会造成浏览器的 CPU 压力。
- **进步**：React 18 支持 **Selective Hydration**（选择性水合），通过 `Suspense` 包裹的部分可以优先渲染并交互。

---

## 4. 实战准则：如何拆分

- **尽量写成 Server Components**：用于获取数据、渲染列表、SEO 内容。
- **精确定位 Client Components**：仅在需要 `useState`、`useEffect`、按钮点击或浏览器 API（如 `window`）时，才使用 `'use client'` 并将其限制在最小的组件范围内。

```tsx
// 例子：RSC 获取数据，Client 组件处理交互
async function Page() {
  const data = await fetchData(); // RSC 直连数据库
  return (
    <div>
      <Nav />
      <ClientButton data={data} /> {/* 只在这里使用客户端 JS */}
    </div>
  );
}
```
