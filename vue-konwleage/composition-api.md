# Composition API（组合式 API）与 `<script setup>`

## 1. 这是什么

Composition API 的目标是：按“功能”组织代码（而不是按 Options API 的 data/methods/computed 分块）。

- 更容易抽离复用逻辑（Composable）
- 更友好 TypeScript 推导

`<script setup>` 是 SFC 的语法糖：

- 更少样板代码
- 编译期宏（如 `defineProps`/`defineEmits`/`defineExpose`）

## 2. 何时用

- 中大型组件、逻辑可复用、TS 项目：优先 Composition API
- 简单展示组件：任选；团队统一即可

## 3. 常用能力

- 状态：`ref/reactive`
- 派生：`computed`
- 副作用：`watch/watchEffect`
- 生命周期：`onMounted/onUnmounted/...`
- 依赖注入：`provide/inject`
- 抽离复用：自定义 composable（`useXxx`）

## 4. Composable（useXxx）写法约定

建议规范：

- 文件名：`useXxx.ts`
- 只暴露“状态 + 动作 + 生命周期需要的钩子”
- 避免直接依赖组件实例（除非明确要做 DOM/组件层能力）

示例：`usePagination` 返回 `page/pageSize/total` 与 `setPage/reset`。

## 5. 常见坑

- `reactive` 解构丢响应性：用 `toRefs`
- 过度 watch：优先 `computed`
- 在 composable 里创建全局单例但没说明：会造成隐式共享状态（需要时建议 Pinia 或显式缓存）

## 6. 最小示例

```vue
<script setup lang="ts">
import { ref, computed, onMounted } from 'vue'

const count = ref(0)
const double = computed(() => count.value * 2)

onMounted(() => {
  // 初始化逻辑
})
</script>

<template>
  <button @click="count++">count: {{ count }}, double: {{ double }}</button>
</template>
```

## 7. 迁移建议（Options -> Composition）

- 先把状态与 computed 搬到 `setup`
- 再把 methods 变成普通函数
- 再把 watch/lifecycle 改成对应的 hooks
- 最后抽离可复用部分到 `useXxx`
