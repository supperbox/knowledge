# 响应式系统（Reactivity）

## 1. 这是什么

Vue 3 的响应式系统用 `Proxy` 做依赖追踪：

- 读（get）会“收集依赖”（track）
- 写（set）会“触发更新”（trigger）

最终效果：数据变 -> 依赖它的计算属性/副作用/组件渲染自动更新。

## 2. 何时用（选型）

- 局部状态：用 `ref`/`reactive`
- 派生状态：用 `computed`
- 监听变化触发副作用：用 `watch`/`watchEffect`

## 3. 常用 API 速查

- `ref(value)`：包装基本类型/对象，访问用 `.value`
- `reactive(object)`：把对象变成响应式代理
- `computed(() => ...)`：只读派生；也可 `computed({ get, set })`
- `watch(source, cb, options)`：精准监听
- `watchEffect(effect, options)`：自动收集依赖
- `toRef(obj, key)` / `toRefs(obj)`：把 reactive 对象字段“拆出来”仍保留响应性
- `unref(x)` / `isRef(x)`：工具函数
- `shallowRef` / `shallowReactive`：只做一层响应式（性能/兼容用）
- `markRaw`：标记对象不做响应式（例如第三方类实例）

## 4. 核心规则（非常重要）

- `ref` 包装的对象在模板里会自动解包，但在 JS/TS 里仍需 `.value`
- `reactive` 适合对象/数组；但“解构”会丢响应性：
  - ✅ `const state = reactive({ a: 1 })`
  - ❌ `const { a } = state`（a 变成普通值）
  - ✅ `const { a } = toRefs(state)`

## 5. 常见坑与排错

- 监听不生效：
  - `watch(obj, ...)` 默认是浅层；要深层用 `{ deep: true }` 或改为监听具体字段
- 监听触发太频繁：
  - 使用 `flush: 'post'`、防抖、或改用 `computed` 减少 watch
- 数组/对象“整体替换”导致引用丢失：
  - 如果把 `reactive` 对象整体替换（`state = ...`）不会生效；用 `ref` 包对象更适合整体替换
- `watchEffect` 里引用了不该引用的值，导致依赖过多：
  - 把不需要追踪的值移出 effect，或用 `watch` 精准指定 source

## 6. 最小示例

```ts
import { ref, reactive, computed, watch } from 'vue'

const count = ref(0)
const state = reactive({ items: [] as string[] })

const itemCount = computed(() => state.items.length)

watch(count, (n, o) => {
  // 适合写：请求、日志、和外部系统同步
})
```

## 7. 实战建议

- 能用 `computed` 就别用 `watch` 维护派生值（少状态、更稳定）
- 复杂表单：建议 `reactive` 聚合；需要整体替换/重置时用 `ref({ ... })`
- 大列表：用 `shallowRef` + 手动替换引用（减少深层追踪）
