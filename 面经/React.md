# React

---

## 综述

### 1、React 的优点和缺点

---

## 性能 / 优化

### 1、如何判断计算是否昂贵？

一般来说，除非创建或循环数千个对象，否则计算可能并不昂贵。<br>
如果想获得更多理论依据，可以添加控制台日志来测量一段代码所花费的时间：

``` js
console.time('filter array');
const visibleTodos = filterTodos(todos, tab);
console.timeEnd('filter array');
```

如果记录的总时间加起来相当大（例如：1ms 或者更多），那么该计算就是昂贵的。<br>
作为实验，可以将计算进行包装 useMemo 以验证该交互的总记录时间是否减少：

``` js
console.time('filter array');
const visibleTodos = useMemo(() => {
  return filterTodos(todos, tab); // Skipped if todos and tab haven't changed
}, [todos, tab]);
console.timeEnd('filter array');
```

可以看到：`useMemo` 不会使第一次渲染更快，它只是会帮助你跳过不必要的更新工作。

### 2、性能测试

- 开发机器可能比用户的机器更快，最好通过人为减速来测试性能。例如：Chrome 为此提供了 CPU 限制选项。
