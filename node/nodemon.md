# nodemon

---

nodemon 是一种工具，可在检测到目录中的文件更改时通过自动重新启动 node 应用程序来帮助开发基于 node.js 的应用程序。

## nodemon 特性

- 自动重新启动应用程序
- 检测要监视的默认文件扩展名
- 默认支持 node，但易于运行任何可执行文件，如 python、ruby、make 等
- 忽略特定的文件或目录
- 监视特定目录
- 使用服务器应用程序或一次性运行实用程序和 REPL
- 可通过 Node require 语句编写脚本
- 开源

**注意：** 本地安装需要在 `package.json` 文件的 `script` 脚本中指定要需要执行的命令

```json
{
  "script": {
    "dev": "nodemon app.js"
  }
}
```

## 使用

nodemon 一般只在开发时使用，它最大的长处在于 watch 功能，一旦文件发生变化，就自动重启进程。

``` sh
# 默认监视当前目录的文件变化
$ nodemon app.js

# 指定主机和端口作为参数，表示在本地 3697 端口启动 node 服务 
$ nodemon app.js localhost 3697
```

## 参数

### watch

监控的文件夹路径或者文件路径。<br>
watch 可以监控多个目录，默认值：'*.*'。<br>
默认情况下，nodemon 监控当前工作目录。如果您想要控制该选项，请使用该选项添加特定路径：--watch

``` sh
# 监控指定文件夹或者文件变化
$ nodemon --watch app --watch libs app.js
```

### ext

默认监听："js, mjs, json"<br>
默认情况下，nodemon 查找扩展名为 .js，.mjs，.coffee，.litcoffee，和 .json 的文件。<br>
使用 -e 或 --ext 指定监听的文件扩展名，如下所示：

``` bash
$ nodemon -e js,pug
```

**优先级：** nodemon 会先读取 `watch` 里面需要监听的文件或文件路径，再从文件中选择监控 `ext` 中指定的后缀名，最后去掉从 `ignore` 中指定的忽略文件或文件路径。

### exec

exec 执行项。若设定了执行项，nodemon 将执行程序而不是 JavaScript 脚本。

``` bash
$ nodemon --exec "python -v" ./app.py
```

### ignore

ignore 忽略项（包括文件、目录或文件名通配符匹配）。<br>
注意，默认情况下，nodemon 会忽略 .git，node_modules，bower_components，.nyc_output，coverage 和 .sass-cache 目录，并添加你的忽略模式到列表中。<br>
将 ignore 置空并不能取消忽略。

``` sh
# 忽略特定的文件
$ nodemon --ignore lib/app.js

# 忽略多个目录
$ nodemon --ignore lib/ --ignore tests/

# 使用通配符匹配（但是一定要引用引号），* 表示该文件夹下的所有后缀为 .js 的文件
$ nodemon --ignore 'lib/*.js'
```

### execMap

execMap 设置运行服务的后缀名与对应的命令。

``` json
{
  "execMap": {
    "js": "npm -v"
  }
}
```

可以用来定义默认可执行文件，如果您使用的语言在默认情况下不受 Node 支持，则此应用特别有用。

``` json
{
  "execMap": {
    "pl": "perl"
  }
}
```

现在运行以下命令，nodemon 将知道将其 perl 用作可执行文件：

``` sh
$ nodemon app.pl
```

### delay

delay 延迟重启时间（毫秒）。延迟重启类似于 JavaScript 函数中的**函数节流**，只在最后一次更改的文件往后延迟重启，以避免了短时间多次重启。

``` bash
$ nodemon --delay 10 app.js
$ nodemon --delay 2.5 app.js
$ nodemon --delay 2500ms app.js
```

### verbose

verbose 设置日志输出模式，true 详细模式

### colours

colours 默认为 true，输出信息颜色标示。

### events

events 表示 nodemon 运行到某些状态时的一些触发事件，总共有五个状态：

- `start`：子进程（即监控的应用）启动
- `crash`：子进程崩溃，不会触发 exit
- `exit`：子进程完全退出，不是非正常的崩溃
- `restart`：子进程重启
- `config:update`：nodemon 的 config 文件改变

### restartable

restartable 设置重启模式。重启的命令，默认是 rs，可以改成你自己喜欢的字符串。

### env

env 运行环境

``` json
{
  "env": {
    "NODE_ENV": "development", // 开发环境
    "PORT": "3000" // 端口号
  }
}
```

---

**优先级：** 本地配置文件 -> nodemonConfig -> 全局配置文件。命令行中指定的参数选项会被本地配置文件覆盖，而在 package.json 中配置的会被命令行覆盖。

每次修改配置文件修改完记得重启一下。
