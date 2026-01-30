# 渲染机制：Virtual DOM + 高效更新

## 1. 这是什么

Vue 组件渲染大致分两步：

1. 编译：模板 -> 渲染函数（render）
2. 运行：render 产生 Virtual DOM（VNode）-> diff -> patch 到真实 DOM

Vue 3 的重点（编译期优化）：

- **编译期标记动态节点（Patch Flags）**
  - 在生成 VNode 时，编译器会根据节点上的动态内容（如 `:class`、`{{ text }}`）打上一个数字标识（Flags）。
  - **效果**：运行时 diff 算法看到标识后，会**精准只对比**发生变化的属性（如只比对 class，跳过 id/title 等静态属性），效率极高。
- **Block Tree（块级更新）**
  - 编译器将模板按照结构动静拆分为“块”。一个 Block 内部会维护一个平坦的“动态节点列表”。
  - **效果**：更新时不再递归遍历整棵树，而是**直接跳过所有静态子树**，只遍历 Block 内部记录的动态节点。Diff 时间复杂度从“全量模板节点数”降为“动态节点数”。
- **静态提升（Static Hoisting）**
  - 将与响应式数据无关的静态节点（及其子树）移出 `render` 函数外。
  - **效果**：组件更新时，静态节点**只会被创建一次**且在后续渲染中直接复用同一个引用，完全消除了重复创建 VNode 的开销。

从而显著减少运行时 diff 与内存占用。

## 2. 你需要知道的“性能开关”

- `v-if`/`v-show` 的选择
- `v-for` 的 key 稳定性
- 避免在模板中频繁调用函数
- 大列表：考虑虚拟列表（组件库/自研）

## 3. 渲染触发来源

- 响应式依赖变化（ref/reactive/computed）
- props 变化
- 父组件重新渲染导致子组件更新（取决于依赖与静态提升）

## 4. 优化手段（实用）

- 把昂贵计算放进 `computed`
- 把不会变化的对象/数组常量移出 render（或用 `markRaw`）
- 列表渲染：使用稳定 key；避免把 `index` 当 key
- 子组件做“输入最小化”：props 尽量传基本类型或稳定引用

## 5. 常见坑

- “为什么我改了对象字段 UI 没更新？”
  - 可能是非响应式对象（markRaw/普通对象）或丢失响应性（解构）
- “列表渲染错乱/复用错误”
  - key 不稳定或 key 冲突
- “渲染频繁导致卡顿”
  - watchEffect 引入过多依赖；模板函数调用；大列表无虚拟化

## 6. 最小示例：避免模板函数调用

```vue
<script setup lang="ts">
import { computed } from 'vue'

const props = defineProps<{ items: { id: string; name: string }[] }>()
const names = computed(() => props.items.map((i) => i.name).join(','))
</script>

<template>
  <div>{{ names }}</div>
</template>
```
