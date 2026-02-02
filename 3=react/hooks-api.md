# Hooks API

Hooks 是 React 在函数组件时代的核心 API：让你在函数里使用状态、生命周期、副作用与复用逻辑。

## 1. useState：局部状态

### 1.1 基本用法

```tsx
const [count, setCount] = useState(0);
```

### 1.2 函数式更新（避免闭包拿到旧值）

```tsx
setCount((prev) => prev + 1);
```

### 1.3 状态结构建议

- 简单优先：能用多个 `useState` 就不要硬塞一个大对象。
- 避免原地修改：数组/对象要返回新引用。

## 2. useEffect：副作用（请求/订阅/DOM）

### 2.1 执行与清理

```tsx
useEffect(() => {
  const id = setInterval(() => console.log("tick"), 1000);
  return () => clearInterval(id);
}, []);
```

### 2.2 依赖数组（Dependencies）怎么写

- `[]`：只在挂载后执行一次（开发环境 StrictMode 可能会“模拟再次挂载”）。
- `[a, b]`：当且仅当 `a/b` 变化时重跑。
- 省略依赖：每次渲染后都执行（通常不推荐）。

经验法则：**effect 内用到的、来自组件作用域的变量/函数，原则上都应该出现在依赖数组里**。

### 2.3 常见反模式

- 把“派生状态同步”写成 effect：例如 `useEffect(() => setB(f(a)), [a])`，通常可直接在渲染中计算或 `useMemo`。
- 依赖数组故意写空来“骗过 lint”：容易产生 bug（读到旧 props/state）。

## 3. useMemo / useCallback：缓存与稳定引用

### 3.1 useMemo（缓存计算结果）

```tsx
const filtered = useMemo(
  () => list.filter((x) => x.name.includes(keyword)),
  [list, keyword],
);
```

适用场景：计算昂贵、依赖稳定、结果会被多次使用。

### 3.2 useCallback（缓存函数）

```tsx
const onSelect = useCallback((id: string) => setSelected(id), []);
```

适用场景：

- 传给 `memo` 子组件的回调
- 作为 effect 依赖希望保持稳定

注意：不要为“所有函数”都 useCallback，缓存也有成本。

## 4. useRef：跨渲染存值 / 访问 DOM

### 4.1 访问 DOM

```tsx
const inputRef = useRef<HTMLInputElement | null>(null);
useEffect(() => {
  inputRef.current?.focus();
}, []);
```

### 4.2 存放不会触发渲染的可变值

```tsx
const latestRequestId = useRef(0);
```

## 5. useReducer：复杂状态更清晰

当状态更新逻辑复杂、需要“事件驱动”更清晰时，用 reducer 会更像一个小型状态机。

```tsx
type State = { count: number };
type Action =
  | { type: "inc" }
  | { type: "dec" }
  | { type: "add"; payload: number };

function reducer(state: State, action: Action): State {
  switch (action.type) {
    case "inc":
      return { count: state.count + 1 };
    case "dec":
      return { count: state.count - 1 };
    case "add":
      return { count: state.count + action.payload };
    default:
      return state;
  }
}

const [state, dispatch] = useReducer(reducer, { count: 0 });
```

## 6. useContext：跨层共享（但要注意重渲染）

Context 适合“低频变化、全局性质”的数据（主题、登录态、i18n）。高频变化更建议外置 store（或拆分 context）。

## 7. React 18 并发相关 Hooks（常用但容易忽略）

### 7.1 useTransition：把更新标记为“可让步”

用于“输入很频繁，但结果渲染很重”的场景（搜索过滤、列表渲染）。

```tsx
const [isPending, startTransition] = useTransition();

function onChange(v: string) {
  setKeyword(v); // 高优先级：输入
  startTransition(() => {
    setQuery(v); // 低优先级：重渲染大列表
  });
}
```

### 7.2 useDeferredValue：延迟一个值

让某个依赖值“慢一点”更新，从而减少高频渲染压力。

## 8. Hooks 使用规则（必须遵守）

1. 只能在顶层调用（不能在循环/条件/嵌套函数中）。
2. 只能在 React 函数组件或自定义 Hook 中调用。
3. 启用 `eslint-plugin-react-hooks`，让规则自动兜底。

## 9. 自定义 Hook：复用逻辑的推荐方式

自定义 Hook 用于封装“状态 + 副作用 + 事件处理”的组合逻辑，命名以 `use` 开头。

```tsx
function useBoolean(initial = false) {
  const [value, setValue] = useState(initial);
  const on = useCallback(() => setValue(true), []);
  const off = useCallback(() => setValue(false), []);
  const toggle = useCallback(() => setValue((v) => !v), []);
  return { value, on, off, toggle };
}
```
