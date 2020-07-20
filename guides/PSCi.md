
PSCi "PureScript Interactive" 是 PureScript 的 REPL。它能够有效探索你的代码，做一些基础测试或者体验新的library。

## 准备开始

使用 `Spago` 配置使用 PSCi：

```bash
$ npm install -g spago       # 安装 Spago

$ cd my-project              # 进入空目录

$ spago init                 # 初始化 spago 环境

$ spago repl                 # 启动 repl
[info] Installation complete.
PSCi, version 0.13.8
Type :? for help

import Prelude

> "hello"
"hello"

> 1 + 2 * 3
7

> import Effect.Console
> log "print this to the screen"
print this to the screen
unit
```

有关开发环境入门的更多信息，请参阅[这篇指南](http://www.purescript.org/learn/getting-started/)。

## 基础使用

在 REPL 中输入表达式并计算：

```bash
> 3 + 4
7
```

使用 `=` 绑定：

```bash
> x = 4
> x + 9
13
```

您还可以定义数据类型、类型类和类型类实例（您可能需要为此使用多行模式）。

```bash
> data Season = Spring | Summer | Autumn | Winter
> class Shout a where shout :: a -> a
> instance shoutString :: Shout String where shout s = s <> "!"
```

## 粘贴模式(`--multi-line-mode`)

输入 `:paste` (或 `:pa`) 进入多行模式（粘贴模式）。使用 `Control-D` 键退出。

```bash
> import Prelude
> :paste
… add :: Int -> Int -> Int
… add = \x y -> x + y
> add 10 20
> (^D)
30
```

[更多例子](https://asciinema.org/a/0y56unmja6fqire01x20zb5xx)

## `purescript-psci-support`

```bash
$ spago repl

PSCi requires the `purescript-psci-support` package to be installed.
You can install it using Spago by adding `"psci-support"` to the list of dependencies in `spago.dhall`.

For help getting started, visit http://wiki.purescript.org/PSCi
```

PureScript 编译器套件（即可执行的 `purus`），与大多数编译器不同，并不附带标准库。在 PureScript 中，甚至 `Prelude` 也是一个普通的模块，就像其他模块一样。相应地，`purus repl`需要安装一个特定的库，以便能够评估repl中的术语。

`purescript-psci-support` 为此定义了 `Eval` 类型类。`Eval` 的实例是为 `Showable` 类型和 `Effect` 提供的，这样我们就可以在 REPL 中评估动作。

## 无 spago 的 PSCi

通过将 PureScript 源文件列表指定为 globs，可以直接运行 PSCI。

```bash
purs repl 'src/**/*.purs' 'path/to/packages/**/*.purs'
```

这表示你已经下载了路径 `/to/packages` 下的 PureScript 源码 (包括 `psci-support`)。

请注意单引号 -- purescript 编译器本身知道如何扩展 globs (`*`) 和递归 globs (`**`)，单引号可以防止 shell 扩展它们。(例如，Bash 默认没有启用递归 glob，不要忘记单引号。)
