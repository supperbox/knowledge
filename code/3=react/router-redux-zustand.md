# 路由与全局状态管理

React 生态提供了丰富的选型，目前推荐：**React Router v6** + **Zustand** (轻量) 或 **Redux Toolkit** (规范)。

---

## 1. React Router (v6+)

v6 引入了大幅简化的 API 和 Hooks。

### 1.1 常用组件

- `BrowserRouter`：入口包裹。
- `Routes` / `Route`：定义匹配规则。
- `Link` / `NavLink`：导航。

### 1.2 常用 Hooks

- `useNavigate()`：执行跳转逻辑。
- `useParams()`：获取路径参数（如 `:id`）。
- `useSearchParams()`：获取/修改 URL 查询参数（`?query=...`）。

### 1.3 嵌套路由示例

```tsx
<Routes>
  <Route path="/" element={<Layout />}>
    <Route index element={<Home />} />
    <Route path="profile/:id" element={<Profile />} />
  </Route>
</Routes>
```

_注：`<Outlet />` 放在 Layout 组件中用于渲染子路由。_

---

## 2. Zustand：现代首选

Zustand 是目前 React 社区最受欢迎的轻量级状态库。

### 2.1 核心优势

- 无需 `Provider` 包裹，即写即用。
- 逻辑在外，UI 在内。
- 支持简单的异步 Action。

### 2.2 代码示例

```ts
import { create } from 'zustand'

const useStore = create((set) => ({
  count: 0,
  inc: () => set((state) => ({ count: state.count + 1 })),
  fetchInfo: async () => {
    const res = await api();
    set({ info: res });
  }
}))

// 组件中使用
function Counter() {
  const { count, inc } = useStore();
  return <button onClick={inc}>{count}</button>;
}
```

---

## 3. Redux Toolkit (RTK)

适合多人写作的大型项目，强制执行单向数据流和不变量更新风格。

- **Slice**：将状态、Action 和 Reducer 封装在一起。
- **Selector**：精确订阅状态（如 `useSelector(state => state.user.name)`）。
- **Dispatch**：触发变更。

---

## 4. 状态选型自测

- **简单全局变量（如主题、i18n）** -> `Context API`。
- **中小型项目、敏捷开发** -> `Zustand`。
- **高并发、超大型、高度规范化项目** -> `Redux Toolkit`。
- **数据获取/缓存管理** -> `TanStack Query (React Query)`。
