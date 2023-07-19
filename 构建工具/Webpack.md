# Webpack

---

## 依赖管理

### 带表达式的 require 语句

如果你的 request 含有表达式（expressions），就会创建一个上下文（context），因为在编译时（compile time）不清楚具体导入哪个模块。

#### context module

它包含目录下所有模块的引用，如果一个 request 符合正则表达式，就能 require 进来。<br/>
该 context module 包含一个 map（映射）对象，会把 requests 翻译成对应模块的 id。<br/>
示例 map：<br>

``` mk
{
  "./table.ejs": 42,
  "./table-row.ejs": 43,
  "./directory/another.ejs": 44
}
```

此 context module 还包含一些访问这个 map 对象的 runtime 逻辑。<br>
这意味着 webpack 能够支持动态地 require，但会导致所有可能用到的模块都包含在 bundle 中。

### require.context

通过 `require.context()` 函数可以创建自己的 context。<br>
可以给这个函数传入三个参数：

- 一个要搜索的目录；
- 一个标记，表示是否还搜索其子目录；
- 一个匹配文件的正则表达式；

Webpack 会在构建中解析代码中的 `require.context()`。<br>
语法如下：

``` js
require.context(
  directory,
  (useSubdirectories = true),
  (regExp = /^\.\/.*$/),
  (mode = 'sync')
);
```

示例：

``` js
require.context('./test', false, /\.test\.js$/);
//（创建出）一个 context，其中文件来自 test 目录，request 以 `.test.js` 结尾。
```

``` js
require.context('../', true, /\.stories\.js$/);
// （创建出）一个 context，其中所有文件都来自父文件夹及其所有子级文件夹，request 以 `.stories.js` 结尾。
```

**注意：**传递给 `require.context()` 的参数必须是字面量（literal）!

#### context module API

一个 context module 会导出一个（require）函数，此函数可以接收一个参数：request。<br>
此导出函数有三个属性：resolve, keys, id。

- `resolve` 是一个函数，它返回 request 被解析后得到的模块 id。
- `keys` 也是一个函数，它返回一个数组，由所有可能被此 context module 处理的请求（参考下面第二段代码中的 key）组成。

如果像引入一个文件夹下面的所有文件，或者引入能匹配一个正则表达式的所有文件，这个功能就会很有帮助：

``` js
function importAll(r) {
  r.keys().forEach(r);
}

importAll(require.context('../components/', true, /\.js$/));
```

```js
const cache = {};

function importAll(r) {
  r.keys().forEach((key) => (cache[key] = r(key)));
}

importAll(require.context('../components/', true, /\.js$/));
// 在构建时(build-time)，所有被 require 的模块都会被填充到 cache 对象中。
```

- `id` 是 context module 的模块 id，它可能在你使用 `module.hot.accept` 时会用到。
