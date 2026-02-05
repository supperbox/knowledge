Vue 框架（以 Vue 3 为主）的核心特性大致可以按“开发体验 + 运行时能力 + 工程生态”分几类。

## 导航（TOC）

- [README.md](./README.md)（知识库索引）
- 核心机制
  - [reactivity.md](./reactivity.md)（响应式系统）
  - [rendering-vdom.md](./rendering-vdom.md)（虚拟 DOM + 高效渲染）
  - [compiler-sfc.md](./compiler-sfc.md)（编译器能力 / SFC）
  - [lifecycle-effects.md](./lifecycle-effects.md)（生命周期与副作用管理）
- 组件开发
  - [components.md](./components.md)（组件化）
  - [template-directives.md](./template-directives.md)（模板语法与指令系统）
  - [composition-api.md](./composition-api.md)（Composition API）
  - [options-api.md](./options-api.md)（Options API）
  - [builtins-transition-teleport-suspense.md](./builtins-transition-teleport-suspense.md)（内置能力）
- 工程与生态
  - [router-pinia.md](./router-pinia.md)（路由与状态管理）
  - [tooling-vite-ts.md](./tooling-vite-ts.md)（工程化与工具链）
  - [ssr-ssg-crossplatform.md](./ssr-ssg-crossplatform.md)（跨端/渲染形态）

- 响应式系统（Reactivity）
  - 数据变化自动驱动视图更新，基于 `Proxy`（Vue 3）实现细粒度依赖追踪。
  - 提供 `ref`/`reactive`/`computed`/`watch` 等能力，适合状态与派生状态管理。

- 组件化（Component-based）
  - UI 拆分为可复用组件，支持 Props/Events/Slots（含具名/作用域插槽）进行组合与扩展。
  - 单文件组件 SFC（`.vue`）把模板、逻辑、样式聚合在一起，利于维护。

- 模板语法与指令系统
  - 声明式模板 + 指令：`v-if`/`v-for`/`v-model`/`v-show`/`v-bind`/`v-on` 等。
  - 事件修饰符、按键修饰符、`class/style` 绑定、列表渲染优化等。

- Composition API（组合式 API）
  - 用 `setup()`/`<script setup>` 组织逻辑，按“功能”而不是“选项”拆分代码，复用更自然（Composable）。
  - 更好支持 TypeScript 类型推导与逻辑抽离。

- Options API（选项式 API，仍支持）
  - 经典的 `data/methods/computed/watch/lifecycle` 写法，适合入门和中小型组件。

- 虚拟 DOM + 高效渲染
  - 基于 Virtual DOM，并结合编译期优化（静态提升、Patch Flag、Block Tree 等）减少运行时开销。
  - 支持按需更新与高性能 diff。

- 编译器能力（Compiler）
  - 模板会被编译成渲染函数；支持编译期优化与更严格的模板检查。
  - SFC 编译支持 `<style scoped>`、CSS Modules、`<script setup>` 宏等。

- 生命周期与副作用管理
  - 组件生命周期钩子（如 `onMounted/onUnmounted` 等），便于管理订阅、定时器、DOM 操作。
  - `watchEffect` 等帮助管理响应式副作用。

- 内置能力：过渡/动画、Teleport、Suspense
  - `<Transition>/<TransitionGroup>`：过渡与列表动画。
  - `<Teleport>`：把组件渲染到 DOM 任意位置（常用于弹窗）。
  - `<Suspense>`：配合异步组件/异步依赖的占位与回退。

- 路由与状态管理生态（官方/事实标准）
  - Vue Router：SPA 路由、嵌套路由、守卫、动态路由等。
  - Pinia：官方推荐状态管理（取代 Vuex 的主流位置），更贴合 Composition API 与 TS。

- 工程化与工具链友好
  - 和 Vite 深度协作：极速热更新、按需编译、ESM 生态。
  - TypeScript 支持成熟，SFC 类型推导更完善。

- 跨端/渲染形态支持
  - SSR（服务端渲染）/ SSG（静态站点生成）能力完善（如结合 Nuxt）。
  - 也可用于小程序/原生（通过社区方案）等多端。
