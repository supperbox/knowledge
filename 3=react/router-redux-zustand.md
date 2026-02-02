# 路由与全局状态管理

## 1. React Router
- **核心组件**：`BrowserRouter`, `Routes`, `Route`, `Link`。
- **Hooks**：`useNavigate`, `useParams`, `useLocation`。
- **动态导航**：通过编程式导航实现业务跳转。

## 2. Redux
- **概念**：Action, Reducer, Store。
- **配套**：`@reduxjs/toolkit` (RTK) 是现代用法，简化大量样板代码。
- **Hooks**：`useSelector`, `useDispatch`。

## 3. Zustand (推荐)
- 轻量级、简单直观。
- 基于 Hooks 设计，无需 Provider 包裹即可使用。

## 4. 状态机思想
- **单向修改**：状态更改必须通过明确的逻辑触发。
- **不可变性**：禁止直接修改状态对象，必须返回新对象（Immutability）。
