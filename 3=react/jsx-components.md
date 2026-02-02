# JSX 与组件化

## 1. JSX 基础

JSX 是 JavaScript 的语法扩展，允许用类似 HTML 的写法描述 React Element。

### 1.1 常见语法规则

- 属性名采用驼峰：`className`、`tabIndex`、`onClick`。
- `style` 接受对象：`style={{ backgroundColor: 'red' }}`。
- 必须有一个根节点：可用 `<>...</>`（Fragment）。
- 在 JSX 中插入 JS 表达式用 `{}`。

### 1.2 JSX 不是字符串模板

JSX 会被编译成 `React.createElement(...)`（或新的 JSX runtime 调用），本质是“描述 UI 的对象结构”。

## 2. 组件的边界：什么该做、什么不该做

### 2.1 组件渲染应尽量“纯”

- ✅ 建议：根据 `props/state` 计算 UI。
- ❌ 避免：在渲染过程中发请求、订阅事件、读写 localStorage、直接操作 DOM。

副作用应集中放在 `useEffect`（见 [生命周期与副作用](lifecycle-effects.md)）。

### 2.2 函数组件 vs 类组件

- **函数组件**：现代主流（Hooks 体系完整、组合性强）。
- **类组件**：主要用于维护历史项目；新代码不推荐。

## 3. Props（输入）

### 3.1 Props 的几个关键点

- 单向数据流：`props` 由父组件拥有，子组件只读。
- Props 应尽量稳定、语义清晰：`onSubmit` 比 `handleSubmit` 更像“对外契约”。

### 3.2 children 是“组合能力”

`children` 让组件像容器一样承载内容，是 React 组合模型的核心。

```tsx
export function Card({
  title,
  children,
}: {
  title: string;
  children: React.ReactNode;
}) {
  return (
    <section>
      <h3>{title}</h3>
      <div>{children}</div>
    </section>
  );
}
```

### 3.3 受控组件（Controlled）

受控组件把输入值放在 state 中，UI 与状态保持一致。

```tsx
function EmailInput() {
  const [email, setEmail] = useState("");
  return <input value={email} onChange={(e) => setEmail(e.target.value)} />;
}
```

## 4. 条件渲染

- `cond ? <A /> : <B />`
- `cond && <A />`（注意：当 `cond` 可能为 `0` 等 falsy 值时要小心）

## 5. 列表渲染与 key（性能与正确性的关键）

### 5.1 为什么 key 很重要

React 对列表的更新是“同级比较”，`key` 帮助 React 判断：

- 这是同一个元素（复用 DOM/组件状态）
- 这是移动（只移动）
- 这是新增/删除（创建/销毁）

### 5.2 key 的选择原则

- ✅ 用稳定唯一的业务 id：`item.id`
- ❌ 不要用数组下标：会导致插入/删除后状态错位、输入框内容串行等

```tsx
items.map((item) => <Row key={item.id} item={item} />);
```

## 6. 组件复用的常见模式

### 6.1 “受控 + 回调”模式（最通用）

```tsx
function Toggle({
  value,
  onChange,
}: {
  value: boolean;
  onChange: (v: boolean) => void;
}) {
  return (
    <button onClick={() => onChange(!value)}>{value ? "ON" : "OFF"}</button>
  );
}
```

### 6.2 透传 DOM 属性（增强可用性）

```tsx
type ButtonProps = React.ButtonHTMLAttributes<HTMLButtonElement> & {
  variant?: "primary" | "ghost";
};

function Button({ variant = "primary", ...rest }: ButtonProps) {
  return <button data-variant={variant} {...rest} />;
}
```

## 7. 常见坑（高频）

- 在渲染期间创建非必要的大对象/函数，导致子组件频繁更新（可用 `useMemo/useCallback` 或拆分组件）。
- 把“派生状态”存进 state：例如 `fullName = first + last`，更适合直接计算或用 `useMemo`。
- key 选错导致输入框内容错位：列表里含输入控件时尤其明显。
