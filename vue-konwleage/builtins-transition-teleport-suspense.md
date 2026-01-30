# 内置能力：Transition / Teleport / Suspense

## 1. `<Transition>` / `<TransitionGroup>`（过渡与动画）

### 用途

- 元素/组件进入与离开时加过渡动画
- 列表插入/删除时加动画（TransitionGroup）

### 核心点

- 基于 CSS class 切换（也支持 JS 钩子）
- 常用 class：`*-enter-active`、`*-enter-from`、`*-enter-to`、`*-leave-active`...

### 常见坑

- 过渡元素必须是“单根”或使用正确的包裹
- 列表动画必须给稳定 key

## 2. `<Teleport>`（传送门）

### 用途

把组件渲染到 DOM 的任意位置（常用于：弹窗、抽屉、全局提示层）。

### 关键点

- `to="body"` 或 `to="#modal-root"`
- 逻辑仍在原组件树中（事件、状态都不变），只是 DOM 位置改变

### 常见坑

- z-index/样式隔离：teleport 到 body 后可能受全局样式影响

## 3. `<Suspense>`（异步依赖占位）

### 用途

- 组件中存在异步依赖（例如 async setup、异步组件）时提供 fallback

### 关键点

- `default` 插槽：真实内容
- `fallback` 插槽：加载中占位

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
