# 内置能力：Transition / Teleport / Suspense

## 1. `<Transition>` / `<TransitionGroup>`（过渡与动画）

### 用途

- 元素/组件进入与离开时加过渡动画
- 列表插入/删除时加动画（TransitionGroup）

### 核心点

- 基于 CSS class 切换（也支持 JS 钩子）
- 常用 class：`*-enter-active`、`*-enter-from`、`*-enter-to`、`*-leave-active`...

### 使用方式

1. **CSS 过渡**：定义 `.v-enter-active` 等类名，Vue 会在对应的生命周期自动切换。
2. **JS 钩子**：使用 `@before-enter`, `@enter`, `@leave` 等监听动画进度，适合 GSAP 等动画库。
3. **初始渲染动画**：加上 `appear` 属性。
4. **TransitionGroup**：必须为子元素提供唯一的 `key`。

### 常见坑

- 过渡元素必须是“单根”或使用正确的包裹
- 列表动画必须给稳定 key

## 2. `<Teleport>`（传送门）

### 用途

把组件渲染到 DOM 的任意位置（常用于：弹窗、抽屉、全局提示层）。

### 关键点

- `to="body"` 或 `to="#modal-root"`
- 逻辑仍在原组件树中（事件、状态都不变），只是 DOM 位置改变

### 使用方式

1. **解决层级遮挡**：在复杂的布局中，将 Modal 或 Popover 直接挂载到 `body` 下，避免父级 `overflow: hidden` 或 `transform` 导致的 CSS 切割。
2. **多重传送**：多个 `Teleport` 可以指向同一个 `to` 目标，内容会按挂载顺序被追加。
3. **禁用传送**：使用 `:disabled="isMobile"` 动态控制是否进行传送（例如在移动端展示在原位，PC 端传送出去）。

### 常见坑

- z-index/样式隔离：teleport 到 body 后可能受全局样式影响

## 3. `<Suspense>`（异步依赖占位）

### 用途

- 组件中存在异步依赖（例如 async setup、异步组件）时提供 fallback

### 关键点

- `default` 插槽：真实内容
- `fallback` 插槽：加载中占位

### 使用方式

1. **组合异步逻辑**：配合 `async setup()`（顶层 await）使用。在异步任务完成前，Vue 会自动渲染 `fallback` 内容。
2. **嵌套 Suspense**：可以嵌套使用，实现局部加载（Internal Loading）和全局加载的平衡。
3. **结合异步组件**：在使用 `defineAsyncComponent` 加载大型包时，包裹 `Suspense` 可以提供默认的加载 UI，提升感知速度。

### 常见坑

- Suspense 的异步边界要清晰，否则 loading 状态难控制

## 4. 最小示例

```vue
<template>
  <Teleport to="body">
    <div class="modal">弹窗内容</div>
  </Teleport>

  <Suspense>
    <template #default>
      <AsyncComp />
    </template>
    <template #fallback>
      <div>加载中...</div>
    </template>
  </Suspense>
</template>
```
