# 工程化：Vite / Next.js / TypeScript

## 1. Vite
- 取代 Webpack 的现代构建工具。
- **快**：基于原生 ESM 和 esbuild。
- **配置**：`vite.config.ts`。

## 2. Next.js
- React 的生产级框架。
- 支持 SSR (服务端渲染)、SSG (静态站点生成)。
- 约定式路由。

## 3. TypeScript 支持
- **FC 定义**：`React.FC<Props>`。
- **Hooks 类型**：`useState<number>(0)`。
- **事件类型**：`React.ChangeEvent<HTMLInputElement>`。

## 4. ESLint & Prettier
- React 官方规则：`eslint-plugin-react-hooks`。
- 保证 Hooks 调用规则的强制执行。
