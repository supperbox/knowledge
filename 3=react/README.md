# React 知识库

这是一个关于 React 及其生态系统的知识库，定位是“可检索、可复用”的实践笔记：既能快速查概念，也能直接复制示例代码。

默认语境：React 18+、React Router v6、TypeScript、现代构建工具（Vite / Next.js）。

## 目录

- [React 概述](react-overview.md)
- [JSX 与组件化](jsx-components.md)
- [Hooks API](hooks-api.md)
- [生命周期与副作用](lifecycle-effects.md)
- [深度解析：Rendering 与 Fiber 架构](rendering-fiber-vdom.md)
- [状态管理与 Context](state-management-context.md)
- [内置能力：Portal / Suspense / StrictMode / Profiler](builtins-portal-suspense.md)
- [样式处理：CSS Modules / CSS-in-JS](styling-css-in-js.md)
- [路由与全局状态：React Router + Redux/Zustand](router-redux-zustand.md)
- [工程化：Vite / Next.js / TypeScript](tooling-vite-nextjs.md)
- [服务端渲染与 RSC (Server Components)](ssr-rsc-nextjs.md)

## 快速索引（按问题找答案）

- “为什么我 setState 了但没更新？”：通常是对象/数组**原地修改**或闭包/依赖问题，优先看 [Hooks API](hooks-api.md) 与 [Rendering 与 Fiber](rendering-fiber-vdom.md)
- “useEffect 什么时候跑、为什么跑两次？”：看 [生命周期与副作用](lifecycle-effects.md)
- “列表 key 到底怎么选？”：看 [JSX 与组件化](jsx-components.md)
- “Context 变动导致全页面重渲染？”：看 [状态管理与 Context](state-management-context.md)

## 术语对齐

- **React Element**：`<App />` 这种 JSX 表达式编译后得到的“描述对象”，不是组件实例。
- **组件（Component）**：函数/类，接收 props，返回 React Element。
- **渲染（render）**：执行组件函数得到 element tree（计算阶段）。
- **提交（commit）**：把变更落到真实 DOM、触发生命周期和 effect（提交阶段）。

## 推荐习惯（和 Vue 对齐的心智模型）

- 组件渲染应尽量“纯”：UI = f(state)。
- 副作用集中管理：创建即注册，卸载即清理（React 对应 `useEffect` cleanup）。
- 状态归位：能局部就局部；能 props 传递就不滥用全局状态。
