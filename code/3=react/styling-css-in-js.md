# 样式处理：CSS Modules / Tailwind / CSS-in-JS

在 React 中，样式选型直接影响包体积、运行时性能和开发体验。

---

## 1. CSS Modules：平衡之选

Vite、Webpack 默认支持。

### 1.1 核心机制

通过构建工具将 `.module.css` 中的类名转换为哈希值，确保样式不会冲突。

### 1.2 组合写法

```tsx
import styles from "./Button.module.css";

// 配合 clsx 或 classnames 库非常方便
return <button className={styles.primary}>提交</button>;
```

---

## 2. Tailwind CSS：效率之选

目前最主流的原子类框架，无需编写独立的 CSS 文件。

- **优势**：
  - 代码极简，无需为类名命名烦恼。
  - 构建产物极小（仅保留用到的 CSS）。
  - 编辑器插件（Tailwind IntelliSense）支持极好。
- **缺点**：JSX 会显得非常“杂乱”。

---

## 3. Styled Components：逻辑之选

典型的 CSS-in-JS 方案，适合根据 `props` 计算样式的场景。

```tsx
const Button = styled.button`
  background: ${(props) => (props.primary ? "blue" : "white")};
  color: ${(props) => (props.primary ? "white" : "black")};
  padding: 10px;
`;
```

- **副作用**：在大规模列表循环中使用可能存在运行时性能损耗（现在由于 React 优化的提升，影响已大幅降低）。

---

## 4. 样式穿透与全局影响

- **CSS Modules**：使用 `:global(.active)` 定义全局类名。
- **动态样式**：
  - React 推荐直接在 JSX 中切换 `className` 而非修改 `style` 对象（性能更优）。
  - _正确示例_：`<div className={isActive ? styles.active : styles.normal} />`。
