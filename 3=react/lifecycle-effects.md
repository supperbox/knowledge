# 生命周期与副作用

## 1. 函数组件的“生命周期”
在 Hooks 时代，生命周期被简化为渲染过程中的副作用同步。

- **挂载/更新**：`useEffect(() => { ... })`
- **挂载一次**：`useEffect(() => { ... }, [])`
- **条件更新**：`useEffect(() => { ... }, [dependencies])`
- **销毁/清理**：`useEffect(() => { return () => { ... } })`

## 2. 副作用管理：创建即注册，卸载即清理
- 定时器、外部订阅、手动 DOM 变动必须在 `useEffect` 的清理函数中执行清除，防止内存泄漏。

## 3. 类组件生命周期（参考）
- `componentDidMount`
- `componentDidUpdate`
- `componentWillUnmount`
- `shouldComponentUpdate` (用于优化)

## 4. 副作用执行时机
- `useEffect` 是异步执行的，在界面渲染完成后调用。
- `useLayoutEffect` 是同步执行的，在界面渲染前且浏览器绘图前调用（慎用）。
