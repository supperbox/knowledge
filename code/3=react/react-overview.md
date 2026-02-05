# React 概述

## 1. React 是什么

React 是一个用于构建用户界面的 JavaScript 库，其核心价值是把 UI 视作状态的函数：

$$UI = f(state)$$

当 `state/props` 改变时，React 重新计算需要更新的部分，并把最小变更提交到真实 DOM。

## 2. React 的核心“心智模型”

### 2.1 声明式 (Declarative) 与 命令式 (Imperative)

- **命令式 (How)**：你需要告诉计算机每一步该怎么做。
  - _例子_：去餐厅吃饭，你进厨房告诉厨师：“先开火，把锅烧热，倒油，把鸡蛋打碎放进去翻炒，加盐，出锅。”
  - _代码体现_：手动操作 DOM（`document.createElement`, `appendChild`, `input.value = ''`）。
- **声明式 (What)**：你只需要告诉计算机“我想要什么结果”。
  - _例子_：去餐厅吃饭，你只需要看菜单对服务员说：“我要一份西红柿炒鸡蛋。”具体的烹饪过程由厨师（React）负责。
  - _代码体现_：通过状态（State）描述 UI。

#### 深度理解声明式：

1. **降低心智负担**：你只需要维护状态（State），React 会自动处理从“旧状态”到“新状态”的视图变动。不再需要写复杂的逻辑来同步 DOM 内容（比如“点击按钮后，隐藏 A 元素，显示 B 元素并清空输入框”）。
2. **UI 是状态的投影**：在 React 中，UI 就像是拍摄状态在某一时刻的照片。不管状态如何变化，只要状态确定，输出的 UI 也是确定的。

#### 代码对比：

**命令式（原生 JS）**

```js
// 逻辑散落在事件监听器中，手动操作 DOM
const btn = document.getElementById("btn");
btn.onclick = () => {
  const content = document.getElementById("content");
  content.innerText = "正在加载...";
  fetchData().then((data) => {
    content.innerText = data;
    content.style.color = "blue";
  });
};
```

**声明式（React）**

```tsx
// 只需定义“加载中”和“成功后”的状态，视图全自动响应
const [data, setData] = useState(null);
const [loading, setLoading] = useState(false);

const load = async () => {
  setLoading(true);
  setData(await fetchData());
  setLoading(false);
};

return <div style={{ color: "blue" }}>{loading ? "正在加载..." : data}</div>;
```

---

### 2.2 组件化 (Component-based)

- 把 UI 切分为可组合的函数（组件），每个组件聚焦一个职责。

### 2.2 单向数据流

- 父组件通过 `props` 把数据传给子组件。
- 子组件通过回调（例如 `onChange`）把“事件”向上通知父组件。
- 状态更新发生在“拥有该状态的组件”中（状态归属清晰）。

### 2.3 为什么要“不可变更新”（Immutability）

React 的更新依赖“引用变化”来触发重新渲染和优化策略（例如 `memo`、依赖数组）。

```ts
// ✅ 正确：返回新引用
setList((prev) => prev.map((x) => (x.id === id ? { ...x, done: true } : x)));

// ❌ 错误：原地修改，可能导致 UI 不更新/优化失效
list[0].done = true;
setList(list);
```

### 2.4 React Element vs 组件实例

- JSX 产物是 **React Element**（描述对象），不是组件实例。
- 组件函数每次渲染都会重新执行；不要把“只想执行一次”的逻辑写在组件函数体里。

## 3. 最小示例（受控组件）

```tsx
import { useState } from "react";

export function SearchBox() {
  const [keyword, setKeyword] = useState("");

  return (
    <label>
      <span>关键词：</span>
      <input
        value={keyword}
        onChange={(e) => setKeyword(e.target.value)}
        placeholder="输入后立即同步到 state"
      />
    </label>
  );
}
```

## 4. 常见误区（快速纠错）

- “组件函数只执行一次”：不对，状态变化会导致组件函数重新执行。
- “useEffect 等于生命周期”：不完全等价，它是**提交后副作用**模型（更像“提交后的钩子集合”）。
- “性能问题就是 VDOM 慢”：多数性能问题来自不必要的重渲染、过大的渲染树、昂贵计算、频繁创建新函数/对象等。

## 5. 学习路线建议（实战顺序）

1. JSX/组件/Props/事件（见 [JSX 与组件化](jsx-components.md)）
2. Hooks 基础与依赖管理（见 [Hooks API](hooks-api.md)）
3. 副作用与清理（见 [生命周期与副作用](lifecycle-effects.md)）
4. 渲染机制、调度与性能（见 [Rendering 与 Fiber](rendering-fiber-vdom.md)）
5. 路由、状态管理、工程化、SSR/RSC（其余文档）
