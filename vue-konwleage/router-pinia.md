# 路由与状态管理：Vue Router + Pinia

## 1. Vue Router（路由）

### 核心概念

- 路由表（path -> component）
- 嵌套路由（layout + children）
- 动态路由（`:id`）
- 路由守卫（鉴权/埋点/数据预取）

### 常见用法

- `useRoute()`：读当前路由参数、query、meta
- `useRouter()`：跳转 `push/replace/back`
- 路由守卫：
  - 全局：`router.beforeEach`
  - 组件内：`onBeforeRouteLeave/onBeforeRouteUpdate`

### 常见坑

- 把“页面状态”放到 query 里导致 URL 过长且难维护：
  - 建议：可分享的才放 URL；其余放 store 或组件状态

## 2. Pinia（状态管理）

### 核心概念

- Store = 全局可共享状态 + actions + getters
- 默认是“响应式”的，配合 Composition API 非常自然

### Store 形态

- setup store（推荐，TS 更好）：

```ts
export const useUserStore = defineStore('user', () => {
  const token = ref('')
  const isLogin = computed(() => !!token.value)
  function setToken(v: string) {
    token.value = v
  }
  return { token, isLogin, setToken }
})
```

### 持久化（常见需求）

- 可用插件实现 localStorage/sessionStorage 持久化
- 注意：敏感信息（token）要评估 XSS 风险与刷新策略

### 常见坑

- 把所有状态都塞进 store：
  - 组件私有状态（弹窗开关、临时输入）不必上全局
- store 相互引用形成循环依赖：
  - 抽离公共逻辑或用 service 层解耦

## 3. 路由 + Pinia 的典型组合

- 守卫中读 userStore 判断登录
- 页面组件只负责展示与交互，数据请求封装在 actions 或 service
