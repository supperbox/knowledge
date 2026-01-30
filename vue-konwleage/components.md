# 组件化（Component-based）：Props / Emits / Slots / Expose

## 1. 这是什么

Vue 的 UI 基本单位是组件：

- 父传子：`props`
- 子通知父：`emit`
- 组合扩展：`slots`
- 暴露实例能力：`defineExpose`/`expose`

## 2. 组件通信方式（优先级建议）

1. 同组件树：`props + emits`（最清晰、可维护）
2. 跨层传递：`provide/inject`（依赖注入，适合主题/表单上下文）
3. 全局共享：Pinia（跨页面/跨模块状态）
4. 事件总线：能不用尽量不用（难追踪）

## 3. Props（输入）

- 单向数据流：子组件不要直接改 props
- TS 推荐：
  - `defineProps<{ value: string; disabled?: boolean }>()`
  - 或运行时校验 `defineProps({ value: { type: String, required: true } })`

常见模式：

- `v-model`：`modelValue` + `update:modelValue`
- 多个 v-model：`v-model:visible` 等

## 4. Emits（输出）

- `defineEmits<{ (e: 'change', v: string): void }>()`
- 事件命名：用动词（`submit`/`change`/`open`），避免把内部细节暴露为事件

## 5. Slots（组合）

- 默认/具名插槽：`<slot />` / `<slot name="footer" />`
- 作用域插槽：子组件把数据“回传”给插槽渲染

## 6. 组件实例与暴露

- 组件默认不应暴露内部实现；需要时用：
  - `<script setup>`：`defineExpose({ focus, reset })`
- 父组件：`const refEl = ref<InstanceType<typeof Child>>()` + `ref="..."`

## 7. 动态组件与异步组件

- 动态：`<component :is="Comp" />`
- 异步：`defineAsyncComponent(() => import('./X.vue'))`

## 8. 常见坑

- 直接修改 props：会被覆盖、并破坏数据源一致性
  - 解决：内部用 `const inner = ref(props.value)` + watch 同步，或用 v-model 标准写法
- 插槽性能问题：插槽内容由父组件渲染，关注父组件更新频率
- 过度 provide/inject：会让依赖关系隐蔽；只用于“真正的上下文”

## 9. 最小示例（v-model 组件）

```vue
<script setup lang="ts">
const props = defineProps<{ modelValue: string }>()
const emit = defineEmits<{ (e: 'update:modelValue', v: string): void }>()
</script>

<template>
  <input
    :value="props.modelValue"
    @input="emit('update:modelValue', ($event.target as HTMLInputElement).value)"
  />
</template>
```
