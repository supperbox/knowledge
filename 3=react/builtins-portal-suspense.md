# 内置组件：Portal / Suspense

## 1. Portal (传送门)
`createPortal(children, domNode)`
- 将组件渲染到主 DOM 树之外的节点（通常在 `body` 根部）。
- 用于处理弹窗、全局通知等对层叠上下文有要求的组件。

## 2. Suspense (异步渲染)
- **用途**：在子组件加载异步数据或异步组件时，显示 `fallback` 占位符。
- **配合**：`React.lazy` 使用实现路由懒加载。
- **并发能力**：React 18 后，Suspense 支持数据获取层面的异步等待。

## 3. StrictMode (严格模式)
- 开发环境下检测副作用、过时 API、意外的重渲染。
- 会触发两次渲染/生命周期，帮助发现潜在 Bug。

## 4. Profiler
- 用于性能检测，分析组件渲染时长。
