# React Hooks

---

## useMemo

- `useMemo` 是一个 React Hook，可以缓存（记忆化）重新渲染之间的计算结果。

``` js
const cachedValue = useMemo(calculateValue, dependencies);
```

- 参数
  - `calculateValue`：计算要缓存的值的函数。它应该是纯的，不应该接受任何参数，并且应该返回任何类型的值。React 将在初始渲染期间调用您的函数。dependencies在下一次渲染时，如果自上次渲染以来没有更改，React 将再次返回相同的值。否则，它将调用calculateValue、返回其结果并存储它以便以后可以重用。
  - `dependencies`：React 将使用比较将每个依赖项与其之前的值进行比较 Object.is。

- 用法
  - 跳过代价高昂的重复计算
  - 跳过组件的重新渲染
  - 记住另一个 Hook 依赖项
  - 记忆一个函数

- 初始渲染时，返回不带参数 useMemo 的调用结果。
- 在下一次渲染期间，它将返回上次渲染中已存储的值（如果依赖项未更改），或者 calculateValue 再次调用并返回已返回的结果 calculateValue。
