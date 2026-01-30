# 模板语法与指令系统

## 1. 这是什么

Vue 模板是“声明式 UI”：你描述状态与结构，Vue 负责把它变成真实 DOM 更新。

模板能力主要来自：

- 插值：`{{ }}`
- 属性绑定：`v-bind` / `:`
- 事件监听：`v-on` / `@`
- 控制结构指令：`v-if`、`v-for`、`v-show`
- 双向绑定：`v-model`

## 2. 常用指令与最佳实践

### 2.1 `v-if` vs `v-show`

- `v-if`：条件为 false 时不渲染（销毁/重建），适合切换频率低、组件昂贵的场景
- `v-show`：始终渲染，仅切换 `display: none`，适合频繁切换

### 2.2 `v-for`

- 必须提供稳定的 `:key`（避免渲染错乱/性能问题）
- key 推荐使用业务唯一 id，不要用 index（除非纯静态列表）

### 2.3 `v-model`

- 表单元素：input/select/checkbox/radio 的值同步
- 组件：约定 `modelValue` + `update:modelValue`

### 2.4 `v-bind`（属性/类/样式）

- `:class` 支持数组/对象：`{ active: isActive }`
- `:style` 支持对象：`{ width: size + 'px' }`

### 2.5 事件与修饰符

- `.stop` `.prevent` `.self` `.once` `.capture` `.passive`
- 按键：`@keyup.enter` 等

## 3. 模板表达式规则

- 只能是“表达式”，不要写复杂语句（if/for/赋值等）
- 复杂逻辑放到 `computed`/方法里：模板更清晰，且可复用可测试

## 4. 自定义指令（Directive）

当你需要直接操作 DOM（聚焦、拖拽、外部库挂载）时使用。

Vue 3 指令钩子：

- `created` / `beforeMount` / `mounted` / `beforeUpdate` / `updated` / `beforeUnmount` / `unmounted`

## 5. 常见坑

- `v-if` 与组件状态：频繁销毁会丢内部状态（输入框内容、滚动位置）
- `v-for` key 不稳定：导致复用错误，出现“内容串行”
- 在模板里直接调用函数：每次渲染都会执行（影响性能）

## 6. 最小示例

```vue
<template>
  <ul>
    <li
      v-for="item in list"
      :key="item.id"
      :class="{ active: item.id === activeId }"
      @click="activeId = item.id"
    >
      {{ item.name }}
    </li>
  </ul>

  <div v-if="activeId">当前：{{ activeId }}</div>
</template>
```
