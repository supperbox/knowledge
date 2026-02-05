# 生命周期与副作用

Hooks 时代的 React 正在淡化“生命周期”的概念，转而强调**“副作用同步” (Synchronization)**。你不再是思考“在组件加载时做什么”，而是思考“这个 Effect 应该与哪些状态保持同步”。

---

## 1. useEffect 的四种“同步”模式

1. **每次渲染后同步**（不推荐）：
   ```ts
   useEffect(() => {
     console.log("渲染了");
   });
   ```
2. **仅在挂载时同步**（Mount）：
   ```ts
   useEffect(() => {
     console.log("组件出生");
   }, []);
   ```
3. **依赖变动时同步**（Update）：
   ```ts
   useEffect(() => {
     console.log("A 变了");
   }, [a]);
   ```
4. **清理机制**（Unmount/Re-sync）：
   ```ts
   useEffect(() => {
     const timer = setInterval(() => {}, 1000);
     return () => clearInterval(timer); // 重要：清理上一次的副作用
   }, [deps]);
   ```

---

## 2. 深度理解：清理函数 (Cleanup)

清理函数不只是在组件“销毁”时执行。它发生在：

1. **下一次 Effect 执行之前**：React 会先清理掉上一次同步留下的痕迹。
2. **组件卸载时**。

### 2.1 为什么要清理？

- **防内存泄漏**：定时器、WebSocket 链接、手动 DOM 监听。
- **防竞态条件 (Race Conditions)**：
  ```ts
  useEffect(() => {
    let ignore = false;
    fetchData().then((res) => {
      if (!ignore) setData(res);
    });
    return () => {
      ignore = true;
    }; // 如果在请求回来前依赖变了，忽略旧请求
  }, [id]);
  ```

---

## 3. 常见误区：什么时候不需要 useEffect

很多初学者把 `useEffect` 当成了万能工具，这会导致代码难以追踪。

1. **不该用于：转换数据**。
   - _错误_：`useEffect(() => { setFullName(first + last) }, [first, last])`。
   - _正确_：直接在函数体内计算 `const fullName = first + last`。
2. **不该用于：处理用户交互事件**。
   - _错误_：点击按钮修改 `state`，然后用 `useEffect` 监测 `state` 变化去发请求。
   - _正确_：直接在按钮的 `onClick` 处理函数里发请求。

---

## 4. 异步时机：useEffect vs useLayoutEffect

1. **useEffect**（常用）：
   - 异步执行，不阻塞浏览器渲染。
   - 流程：状态更新 -> 渲染 -> **浏览器绘制屏幕** -> 执行 useEffect。
2. **useLayoutEffect**（慎用）：
   - 同步执行，会阻塞浏览器渲染。
   - 流程：状态更新 -> 渲染 -> **执行 useLayoutEffect** -> 浏览器绘制屏幕。
   - _场景_：需要在浏览器绘图前读取 DOM 布局、修改 DOM 以防止视觉闪烁（如测量元素高度后立即调整滚动位置）。

---

## 5. 开发环境的“双重执行” (Strict Mode)

在 `React.StrictMode` 下，React 故意在挂载时运行两次 Effect：

- **挂载 -> 卸载 -> 再次挂载**。
- **目的**：强迫开发者检查副作用清理逻辑是否完整。如果你的 Effect 运行两次就坏了，说明你的代码存在逻辑 Bug（通常是漏了清理函数）。

---

## 6. 类组件生命周期对照表

| 类组件钩子              | Hooks 等价实现                                   |
| :---------------------- | :----------------------------------------------- |
| `constructor`           | 直接在函数体内写逻辑（或使用 `useState` 初始化） |
| `componentDidMount`     | `useEffect(..., [])`                             |
| `componentDidUpdate`    | `useEffect(..., [deps])`                         |
| `componentWillUnmount`  | `useEffect(() => { return () => {清理} }, [])`   |
| `shouldComponentUpdate` | `React.memo(Component, compareFunction)`         |
