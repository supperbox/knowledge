# 编译器能力（Compiler）与单文件组件（SFC）

## 1. 这是什么

Vue 的 `.vue` 单文件组件会被编译器拆成三部分：

- Template：模板 -> render 函数
- Script：逻辑（Vue 3 推荐 `<script setup>`）
- Style：样式（支持 scoped、CSS Modules 等）

## 2. `<script setup>` 的“编译期宏”

常用：

- `defineProps()` / `defineEmits()` / `defineExpose()`
- `withDefaults(defineProps<...>(), defaults)`
- `defineOptions({ name: 'Xxx' })`（部分工具链支持）

特点：这些不是运行时函数，编译器会特殊处理。

## 3. `<style scoped>` 原理

- 编译器会为组件生成一个 scopeId
- 给模板元素加 attribute（例如 `data-v-xxxx`）
- 把 CSS 选择器也改写为带 scopeId

注意：

- scoped 不是“真正的 CSS 隔离”，`deep` 仍可穿透

## 4. CSS Modules

- `<style module>` 会把 class 映射成对象：`$style.xxx`
- 适合避免 class 冲突、做组件库

## 5. 模板编译限制与能力

- 模板表达式是 JS 表达式子集（不写复杂语句）
- 编译期静态提升、patch flag 标记，提高性能

## 6. 常见坑

- scoped 下第三方组件样式覆盖困难：
  - 用 `:deep()` 或提升到全局样式
- `<script setup>` 中变量只要在模板使用就会变成渲染依赖：
  - 避免把无关数据放进同一个组件里导致频繁更新

## 7. 最小示例：scoped + deep

```vue
<style scoped>
:deep(.third-party-class) {
  color: red;
}
</style>
```
