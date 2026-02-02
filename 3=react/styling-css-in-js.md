# 样式处理：CSS Modules / CSS-in-JS

## 1. CSS Modules (经典推荐)
- 文件命名：`MyComponent.module.css`。
- **作用**：自动生成局部哈希类名，解决命名冲突。
- **用法**：`import styles from './My.module.css';`

## 2. Styled Components (CSS-in-JS)
- 允许在 JS 中编写真实的 CSS。
- 支持 Props 动态样式。
- 虽然灵活，但在大型应用中可能有少许运行时性能损耗。

## 3. Tailwind CSS (现代主流)
- 工具类优先。
- 无需离开 HTML/JSX 即可快速编写 UI。

## 4. 内联样式 (Inline Styles)
- `style={{ color: 'red' }}`。
- **限制**：不支持伪类、媒体查询。
