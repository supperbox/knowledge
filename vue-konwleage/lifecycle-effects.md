# 生命周期与副作用管理

## 1. 这是什么

生命周期用于在组件“创建/挂载/更新/卸载”的不同阶段执行逻辑。
副作用（side effect）指：与渲染无关但会产生外部影响的操作，例如：

- 发请求、订阅事件、开定时器、读写 LocalStorage、操作 DOM、监听窗口 resize

Vue 3（Composition API）常用生命周期：

- `onMounted`：DOM 已挂载
- `onUnmounted`：组件卸载，清理资源
- `onUpdated`：组件更新后

## 2. 副作用管理的基本原则

- “创建即注册，卸载即清理”
- 把副作用放在生命周期里，而不是散落在 computed/watch 里

## 3. watch / watchEffect 的清理

- `watchEffect` 支持返回清理函数：

```ts
watchEffect((onCleanup) => {
  const id = setInterval(() => {}, 1000)
  onCleanup(() => clearInterval(id))
})
```

## 4. 请求并发与取消（建议策略）

- 快速输入搜索：
  - 防抖 + 只处理最后一次响应
- 路由切换：
  - 在 `onUnmounted` 或 `onBeforeRouteLeave` 取消请求/忽略响应

## 5. 常见坑

- 忘记清理：事件监听、Timer、WebSocket，导致内存泄漏和重复执行
- 在 `onMounted` 做初始化，但 props 可能后续变化：
  - 需要 `watch` props 或在路由进入钩子重新加载
- `watchEffect` 依赖过多导致频繁触发：
  - 改用 `watch` 精准 source

## 6. 最小示例：注册/清理事件

```ts
import { onMounted, onUnmounted } from 'vue'

function onResize() {
  // ...
}

onMounted(() => window.addEventListener('resize', onResize))
onUnmounted(() => window.removeEventListener('resize', onResize))
```
