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

- **“创建即注册，卸载即清理”**
  - 任何占用外部资源的操作（如 `addEventListener`、定时器、WebSocket），必须在生命周期或副作用钩子中成对出现。
- **“重心回归生命周期，而非散落在 watch/computed”**
  - 将逻辑锚定在明确的组件阶段，而非让逻辑随响应式数据“乱跳”。

### 2.1 补充说明：为什么不要散落在 computed/watch？

1. **`computed` 必须是纯函数**
   - **错误做法**：在 `computed` 里修改其他响应式变量或直接发送 API 请求。
   - **后果**：`computed` 具有**惰性（Lazy）**和**缓存性**。如果没人读取该计算属性，里面的副作用可能永远不触发；或者因为重复读取导致副作用多次触发，逻辑不可控。
   - **正解**：`computed` 仅用于根据已有数据派生新数据。

2. **过度依赖 `watch` 会导致“响应式面条代码”**
   - **错误做法**：为了实现“进入页面加载数据”，不写 `onMounted` 而是给 `userId` 加个 `immediate: true` 的 `watch`，并在回调里顺便初始化一堆 DOM 插件。
   - **后果**：当逻辑变复杂（多个 watch 互相触发），追踪代码执行顺序会变得极其痛苦。副作用变得“隐匿”，甚至在组件卸载后因为某些异步逻辑仍在运行。
   - **正解**：
     - 若副作用是**组件级别**的（如：进入页面开启定时器），应明确写在 `onMounted`。
     - 若副作用是**数据驱动**的（如：ID 变了才刷新列表），才使用 `watch`。

3. **防止“只放火不灭火”**
   - 散落在各处的副作用极易遗忘 `onUnmounted` 的清理逻辑，导致内存泄漏或“幽灵回调”（组件销毁了，回调还在跑）。

## 3. watch / watchEffect 的清理

- `watchEffect` 支持返回清理函数：

```ts
watchEffect((onCleanup) => {
  const id = setInterval(() => {}, 1000);
  onCleanup(() => clearInterval(id));
});
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
import { onMounted, onUnmounted } from "vue";

function onResize() {
  // ...
}

onMounted(() => window.addEventListener("resize", onResize));
onUnmounted(() => window.removeEventListener("resize", onResize));
```
