# 状态管理与 Context

React 中的状态管理可以分为三个层级：**局部状态 (Local)**、**组件树状态 (Shared)**、**全局状态 (Global)**。

---

## 1. 状态提升 (Lifting State Up)

这是 React 处理组件间通信的首选方案。

- **原则**：如果两个组件需要访问同一个数据，将该数据提升到它们最近的共同父组件中。
- **优点**：数据流清晰，符合“单向数据流”原则。

---

## 2. Context API：跨层级传递

Context 旨在解决 **Prop Drilling**（属性钻取）问题，即数据需要穿透多层不相关的组件才能到达目的地。

### 2.1 基础用法

```tsx
const ThemeContext = createContext("light");

function App() {
  return (
    <ThemeContext.Provider value="dark">
      <Toolbar />
    </ThemeContext.Provider>
  );
}

function Toolbar() {
  const theme = useContext(ThemeContext);
  return <div>当前主题: {theme}</div>;
}
```

### 2.2 性能陷阱与优化建议

**坑位**：只要 Context 的 `value` 发生变化，**所有**通过 `useContext` 订阅了该 Context 的子组件都会强制重渲染，且默认不受 `React.memo` 限制。

**优化手段**：

1. **Context 拆分**：不要把所有全局状态塞进一个 `BigContext`。将“用户信息”和“界面主题”分开。
2. **组合组件 (Composition)**：有时候你不需要 Context，只需要把组件作为 React Node 传下去。
3. **Memo 化 Value**：确保 Provider 的 `value` 引用稳定。
   ```tsx
   const value = useMemo(() => ({ user, logout }), [user]);
   return <AuthContext.Provider value={value}>{children}</AuthContext.Provider>;
   ```

---

## 3. 黄金组合：Context + useReducer

这种模式在不引入 Redux 的情况下，能很好地处理复杂的业务逻辑。

- **Reducer**：负责定义“状态如何变”。
- **Context**：负责将“状态”和“派遣函数 (dispatch)”传递给深层组件。

```tsx
// 1. 定义 Reducer
function taskReducer(state, action) { ... }

// 2. 创建 Context
const TaskContext = createContext(null);
const DispatchContext = createContext(null);

// 3. 封装 Provider 组件
export function TaskProvider({ children }) {
  const [tasks, dispatch] = useReducer(taskReducer, []);
  return (
    <TaskContext.Provider value={tasks}>
      <DispatchContext.Provider value={dispatch}>
        {children}
      </DispatchContext.Provider>
    </TaskContext.Provider>
  );
}
```

---

## 4. 什么时候该选三方库 (Redux/Zustand)？

虽然 Context 很强大，但以下场景建议使用第三方状态库：

1. **状态更新非常频繁**（如每秒多次更新的位置信息、表单输入）。
2. **需要强大的调试工具**（Redux DevTools）。
3. **逻辑极其复杂**，需要中间件（Middleware）处理异步。
4. **性能极其敏感**：Zustand/Redux 等库有更精细的订阅机制，能避免 Context 的“全量重渲染”问题。
