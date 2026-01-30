# Options API（选项式 API）

## 1. 这是什么

Options API 通过选项块组织组件：

- `data`（状态）
- `computed`（派生）
- `methods`（行为）
- `watch`（监听）
- 生命周期钩子（`mounted` 等）

## 2. 何时用

- 团队已有大量 Options 组件、需要统一风格
- 组件很小、逻辑简单、开发者更熟悉该模式

## 3. 核心概念

- `this` 指向组件实例（在 TS 中类型推导相对麻烦）
- 数据/方法都挂在实例上，容易“全局可见”，需要自律避免耦合

## 4. 常用片段

- props/emit：`props: { ... }`、`emits: ['change']`
- 计算属性：`computed: { fullName() { ... } }`
- 监听：`watch: { keyword(n) { ... } }`

## 5. 常见坑

- TS 类型：`this` 类型很容易变得不精确（建议 Composition + `<script setup>`）
- 逻辑复用：mixins 会带来命名冲突与隐式依赖；更建议 composable
- `data` 必须是函数返回对象（避免实例共享）

## 6. 最小示例

```ts
export default {
  props: { init: { type: Number, default: 0 } },
  data() {
    return { count: this.init }
  },
  computed: {
    double() {
      return this.count * 2
    },
  },
  methods: {
    inc() {
      this.count += 1
    },
  },
}
```

## 7. 与 Composition API 共存策略

- 新增功能优先 Composition（更好维护）
- 老组件只在大改时迁移，避免纯为风格而迁移
