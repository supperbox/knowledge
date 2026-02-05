# Vue 中的 CSS 样式处理

本文详细介绍 Vue（尤其是 Vue 3 + Vite）中提供的样式方案，重点分析 **CSS Modules** 与 `scoped` 样式的区别。

## 1. CSS Modules

### 什么是 CSS Modules？

CSS Modules 不是一种新的 CSS 语法，而是一种在编译阶段将 CSS 类名**哈希化（Hashing）**的自动处理方式。它能确保类名在全局是唯一的，从而彻底解决命名冲突问题。

### 在 Vue 中的用法

只需在 `<style>` 标签上添加 `module` 属性：

```vue
<template>
  <!-- 1. 通过 $style 访问生成的哈希类名 -->
  <p :class="$style.red">Hello World</p>
</template>

<style module>
.red {
  color: red;
}
</style>
```

编译后，HTML 会变成类似：
`<p class="_red_1abc2_5">Hello World</p>`

### 进阶用法

#### 自定义模块名

如果有多个 `<style module>`，可以为它们命名：

```vue
<template>
  <p :class="myClasses.blue">Custom Module Name</p>
</template>

<style module="myClasses">
.blue {
  color: blue;
}
</style>
```

#### 在 Composition API 中使用

通过 `useCssModule` 获取类名对象：

```ts
import { useCssModule } from "vue";

const style = useCssModule(); // 默认获取 <style module>
const customStyle = useCssModule("myClasses"); // 获取命名的 module
```

---

## 2. CSS Modules vs Scoped CSS

| 特性         | Scoped CSS (`<style scoped>`)                                    | CSS Modules (`<style module>`)          |
| :----------- | :--------------------------------------------------------------- | :-------------------------------------- |
| **原理**     | 使用 PostCSS 给 DOM 加 `data-v-xxx` 属性，通过属性选择器限制范围 | 将类名直接改写为全局唯一的哈希字符串    |
| **访问方式** | 正常的 CSS 类名写法                                              | 必须通过 `$style` 或变量访问            |
| **灵活性**   | 较低（深层选择器需 `:deep()`）                                   | 极高（类名即变量，可轻松传递给子组件）  |
| **性能**     | 运行时几乎无开销                                                 | 运行时需通过 JS 映射类名，略有开销      |
| **适用场景** | 大多数常规开发（语法自然）                                       | 复杂组件库、需要将类名作为 Props 传递时 |

---

## 3. 辅助功能

### `:deep()` 深层选择器

在 `scoped` 样式中修改子组件内部样式：

```css
.parent :deep(.child) {
  /* 即使 .child 在子组件内也会生效 */
}
```

### `:global()` 全局选择器

在局部样式块中定义全局样式：

```css
:global(.full-screen) {
  width: 100vw;
}
```

### `v-bind()` 在 CSS 中使用响应式变量

Vue 3 的黑科技，直接在 CSS 中引用脚本里的变量：

```vue
<script setup>
const themeColor = "red";
</script>

<style scoped>
.text {
  color: v-bind(themeColor);
}
</style>
```

**原理**：Vue 会生成一个自定义 CSS 变量（CSS Variable），并通过内联样式动态更新该变量的值。

---

## 4. 最佳实践建议

1. **优先使用 `scoped`**：它对开发者更友好，符合传统的 CSS 编写习惯。
2. **大型组件库选 CSS Modules**：当你的组件会被他人引用，且需要高度防止样式污染或提供类名配置接口时，CSS Modules 更严谨。
3. **善用 `v-bind()`**：处理动态主题或根据状态改变颜色/高度时，比维护大量的 `:style` 对象更优雅。
