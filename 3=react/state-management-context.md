# 状态管理与 Context

## 1. 状态提升 (Lifting State Up)
将状态移动至最近的公共父组件，实现组件间数据共享。

## 2. Context API
用于解决“Prop Drilling”问题（跨多层级传递数据）。
- `createContext`：创建上下文。
- `Provider`：提供者。
- `useContext`：消费者。

## 3. 适用场景
- 全局主题、用户登录信息、国际化配置等。
- **注意**：Context 变化时，所有消费该 Context 的组件都会重渲染，不适合高频变化的数据。

## 4. 搭配 useReducer
实现类似 Redux 的“状态 + 分发”模式，无需第三方库即可处理复杂状态。
