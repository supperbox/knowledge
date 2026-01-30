# 工程化与工具链：Vite + TypeScript

## 1. Vite 的核心特点

- Dev：基于原生 ESM，启动快、按需编译
- HMR：热更新粒度细，体验好
- Build：使用 Rollup 打包（可代码分割、Tree-shaking）

## 2. TS 在 Vue 3 的最佳实践

- 组件用 `<script setup lang="ts">`
- Props/Emits 强类型：`defineProps<T>()`、`defineEmits<T>()`
- 组合函数（composable）输出明确类型

## 3. 常见工程配置点（概念级）

- `vite.config.ts`：alias、proxy、plugins、build 配置
- `tsconfig.json`：路径别名、类型检查策略
- ESLint/Prettier：统一风格，减少低级问题

## 4. 开发体验能力

- 自动导入（unplugin-auto-import 等）
- 组件自动注册（unplugin-vue-components 等）
- 环境变量：`import.meta.env` + `.env.*`

## 5. 常见坑

- 生产/开发环境变量不一致：
  - 约定前缀（常见 `VITE_`）并统一管理 `.env.*`
- alias 与 TS paths 不一致：
  - `vite` alias 与 `tsconfig` paths 需要同步
- CJS/ESM 依赖兼容：
  - 某些库需要 `optimizeDeps` 或替换版本

## 6. 建议的项目分层

- `src/api`：请求封装
- `src/stores`：Pinia stores
- `src/router`：路由
- `src/views`：页面
- `src/components`：通用组件
- `src/utils`：工具
