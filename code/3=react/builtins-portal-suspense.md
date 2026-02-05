# 内置组件：Portal / Suspense / Fragment

React 提供了一些内置组件用于处理特殊的 DOM 结构、异步逻辑和性能监控。

---

## 1. Portal：传送门

`createPortal(children, domNode)` 允许你将组件渲染到父组件 DOM 层次结构之外的 DOM 节点上。

### 1.1 核心用途

- **解决层叠上下文问题**：Modal、Tooltip、Popover 如果放在组件深处，可能会被父元素的 `overflow: hidden` 或 `z-index` 裁剪/遮挡。
- **组件分层**：将全局通知挂载到 `document.body` 或 `#modal-root`。

### 1.2 关键特性

- **事件冒泡**：尽管 Portal 可以被渲染在 DOM 树的任何地方，但它在 React 树中仍然保留在原本的位置。即：Portal 内部触发的点击事件，依然能被 React 父组件捕获。

---

## 2. Suspense：异步等待

`Suspense` 能够让你在子组件完成“某些动作”（加载代码或数据）之前，显示一个加载状态（Loading UI）。

### 2.1 路由懒加载 (Code Splitting)

```tsx
const HugeComponent = React.lazy(() => import("./HugeComponent"));

function App() {
  return (
    <Suspense fallback={<LoadingSpinner />}>
      <HugeComponent />
    </Suspense>
  );
}
```

### 2.2 数据获取 (React 18+)

开启了并发模式后，一些数据获取库（如 Relay、SWR、React Query）可以与 Suspense 配合。在数据返回前状态会保持在 `fallback`。

---

## 3. Fragment：片段

用于包裹多个元素而不引入额外的独立 DOM 节点。

- **语法**：`<React.Fragment>` 或简写 `<>...</>`。
- **唯一属性**：简写不支持属性，但完整写法支持 `key`。
  ```tsx
  {
    list.map((item) => (
      <React.Fragment key={item.id}>
        <dt>{item.term}</dt>
        <dd>{item.description}</dd>
      </React.Fragment>
    ));
  }
  ```

---

## 4. StrictMode：严格模式

`<StrictMode>` 是一个开发工具，不渲染任何可见 UI，仅用于突出显示潜在问题。

- **双重渲染**：在开发环境下，React 会触发两次组件渲染和 Effect 挂载，帮助发现缺乏清理函数的副作用。
- **检测过时 API**：如 `string refs` 或已经废弃的生命周期。

---

## 5. Profiler：性能分析

用于测量 React 应用渲染的频率以及渲染带来的“开销”。

```tsx
<Profiler
  id="App"
  onRender={(id, phase, actualDuration) => {
    console.log(`${id} 组件在 ${phase} 阶段耗时 ${actualDuration}ms`);
  }}
>
  <Navigation />
</Profiler>
```

- **id**：标识符。
- **onRender**：当 React 提交更新时调用的回调函数。
