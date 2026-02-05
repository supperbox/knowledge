# 工程化：Vite / Next.js / TypeScript

React 项目早已告别了繁琐的配置，现代项目通常基于成熟的工程模板。

---

## 1. Vite：现代开发引擎

- **原因**：开发启动几乎秒开，热更新（HMR）稳定且极快。
- **常用插件**：`@vitejs/plugin-react-swc` 使用 Rust 编写的编译器，比 Babel 更快。

---

## 2. Next.js：全栈标准

目前 React 官方推荐的生产环境框架。

- **约定式路由**：`app/` 文件夹下的文件夹即路由。
- **混合渲染**：支持首屏 SSR，内部动态 CSR。
- **自动优化**：自动优化图片（Next Image）、字体（Next Font）。

---

## 3. TypeScript 实战技巧

TypeScript 与 React 深度融合，能够解决 90% 的运行时 Bug。

### 3.1 常用类型定义

```tsx
interface Props {
  title: string;
  count?: number; // 可选
  children: React.ReactNode; // 插槽内容
  onClick: (id: number) => void; // 函数回调
}

const MyComp: React.FC<Props> = ({ title, onClick }) => { ... }
```

### 3.2 钩子泛型

- `useState<User | null>(null)`。
- `useRef<HTMLDivElement>(null)`。

---

## 4. 提交规范与协作工具

- **Husky + Lint-staged**：在 `git commit` 前强制进行样式和类型检查。
- **ESLint**：必须包含 `react-hooks/rules-of-hooks` 以防止钩子逻辑失效。
