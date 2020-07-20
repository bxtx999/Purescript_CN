PureScript 社区已经在一定程度上对 NodeJS 工具集进行了标准化：我们使用 Pulp 、Browserify 和 Gulp 等工具。这些工具都是在NodeJS上运行并使用NPM安装，同时使用Bower进行包管理。然而，有一些用户不愿意或者不能在他们的机器上运行 NodeJS。在这篇短文中，我将演示如何在命令行上直接使用 PureScript 编译器，以及如何使用标准工具自动化一些流程。

#### 创建项目

为您的项目创建目录，其中 `src/` 和 `test/` 分别存放源文件和测试文件。

```bash
$ mkdir purescript-without-node
$ cd purescript-without-node
$ mkdir src/ test/
```

按照下面的代码创建 `src/Main.purs` 文件：

```purescript
module Main where

import Effect (Effect)
import Effect.Console (log)
import Data.Unit (Unit)

main :: Effect Unit
main = log "Hello, World!"
```

#### 安装依赖包

我们推荐您手动安装依赖包，而不是使用 Bower 命令。首先就需要通过 `git` 命令来获取源码。针对上面的代码，我们需要为 `purescript-console` 安装 `purescript-effect` 和 `purescript-prelude` 这两个依赖包。

```bash
$ mkdir dependencies/
$ git clone --branch v4.1.0 git@github.com:purescript/purescript-prelude.git dependencies/purescript-prelude/
$ git clone --branch v2.0.0 git@github.com:purescript/purescript-effect.git dependencies/purescript-effect/
$ git clone --branch v4.1.0 git@github.com:purescript/purescript-console.git dependencies/purescript-console/
```

请注意，如果没有 Bower 作为我们的依赖管理器，就必须手动安装所有的依赖，包括过渡性的依赖。此外，还需要手工管理版本。

#### 编译源码

现在我们可以编译我们的项目了。 必须使用 `purs compile` 编译所有源码。最简单就通过以下代码编译：

```bash
$ purs compile src/Main.purs 'dependencies/purescript-*/src/**/*.purs'
```

注意：当 PureScript 文件使用 `foreign import`时，FFI 文件会被自动找到，不需要使用上面的代码。

```bash
Compiling Effect
Compiling Effect.Class
Compiling Effect.Console
...
```

`purs` 会将CommonJS模块的集合写入输出目录。你可以在 NodeJS 中使用这些 CommonJS 模块，但你更有可能希望将它们打包起来在浏览器中使用。

#### 为浏览器打包 JavaScript

我们使用 `purs bundle` 命令来打包生成 JavaScript 代码。

`purs bundle` 将 CommonJS 模块文件作为输入，并生成一个单一的 JavaScript 文件。

运行 `purs bundle` 时，需要使用以下参数：

```bash
$ purs bundle output/*/{index,foreign}.js --module Main --main Main
```

`--module` 参数指定入口模块(entry-point module)，它将被用来确定可以删除的过期代码。就我们的目的而言，我们只关心与主要的 PureScript模块对应的 CommonJS 模块及其转义依赖的捆绑。

`--main`参数将使输出的 JavaScript 文件成为可执行文件，通过在 JavaScript 中添加一行代码运行指定模块的主函数。

如果一切正常，你应该看到大约 100 行 JavaScript 代码。通过 `--output/-o` 参数可以将这些代码重定向到指定文件。

你应该可以使用 NodeJS 执行生成的 JavaScript 文件。如果你的 PureScript 代码使用了需要 NPM 模块的 FFI 文件，那么在浏览器中运行之前，你需要使用 Webpack 或 Browserify 进行打包。

#### 结语

我已经展示了如何在不使用 NodeJS 生态系统的工具的情况下开始使用 PureScript。显然，对于大型项目来说，用这种方式管理库的依赖性需要一些工作，这也是 PureScript 社区决定重用现有工具的原因。然而，如果你出于任何原因需要避开这些工具，可以在没有这些工具的情况下编写一些标准任务的脚本。
