# 项目

这个文件夹是为了：

- 巩固自己对如何根据现代 FP 架构原理编写 FP 程序的理解。
  - 解释设计一些程序背后的全过程，从一个人想采用的属性开始。这将包括图表和其他解释。
  - 比较人们可以结构 FP 程序的各种方法，并分析它们的优点/缺点和其他权衡。这里涵盖了以下几种方法。
    - ReaderT 方法(所有文件夹)
    - 运行方式（所有文件夹）
    - 其他方法（仅在随机数游戏文件夹中）
  - 解释并展示如何测试和衡量我们的代码
- 向读者介绍一些“现实世界”中常用的库
  - 在这些项目中使用之前，我们会先介绍/查看我们将在以后的项目中使用的库
- 帮助读者确定他们觉得最舒服的方法（没有 "唯一的方法"）

注意：`src` 文件夹中的每个“项目”文件夹在 `test` 和 `benchmark` 文件夹中都有对应的文件夹。读取一个项目的文件时，先读取其 `src` 文件夹的内容，再读取其 `test` 文件夹的内容，然后再读取其 `benchmark` 文件夹的内容。

## 其他示例项目

- [hdgarrood's 'MultiPac' project](https://github.com/hdgarrood/multipac)
- [thomashoneyman's 'Real World App' project](https://github.com/thomashoneyman/purescript-halogen-realworld)
- [jaspervdj's 'Beeraffe' game](https://github.com/jaspervdj/beeraffe/)

### 非 JS 后端项目

- [Lambda Lantern](https://lettier.itch.io/lambda-lantern)

## 确认术语

在概述程序的设计过程时，我们将使用以下术语来指代我们程序的各个层次：

| Layer Level | Onion Architecture Term | General idea |
| - | - | - |
| Layer 4 | Core | Strong types with well-defined properties and their pure, total functions that operate on them
| Layer 3 | Domain | the "business logic" code which uses effects
| Layer 2 | API | the "production" or "test" monad which "links" these effects/capabilties to their implementations: <ul><li>a newtyped `ReaderT` and its instances</li><li>the `Free`/`Run` monad's language and its interpretation</li></ul>
| Layer 1 | Infrastructure | the platform-specific framework/libraries we'll use to implement some special effects/capabilities (i.e. `Node.ReadLine`/`Halogen`/`StateT`)
| Layer 0 | Main<br>(no equivalent onion term) | the program's entry point<br>the "base" monad that runs the program (e.g. production: `Effect`/`Aff`; test: `Identity`)

## 函数库概览

以下是我们目前介绍的库列表，并在此进一步说明。这个列表可能会随着时间的推移而增加。

- `Node.ReadLine` for console-based programs (`purescript-node-readline`)
- `Halogen` for web-based programs (`purescript-halogen`)
- `Aff` for using asynchronous effects (`purescript-aff`)
- `Yargs` for command-line parsing (`purescript-yargs`)
- `Node.FS` for reading/writing files (`purescript-node-fs-aff`)
- `StringParers` for parsing files' contents (`purescript-string-parsers`)
- `Tree` for producing trees (`purescript-tree`)

## 示例项目

- (Easy) "Guess the Random Number" game.
- (Easy-ish) "Table of Contents Generator" program for this specific repository.

## 协议

所有的代码遵循 MIT 协议。

```bash
Copyright 2019 Jordan Martinez and Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
```

## 编译指令

### 配置文件夹

因为这个文件夹包含到 JS 库的绑定，所以我们需要在 purescript 绑定工作之前通过NPM安装它们。按照下列指示

1. 安装 NPM 依赖

    ```bash
    # Benchmark.js - https://benchmarkjs.com/
    npm install benchmark
    ```

1. 使用 `spago` 编译代码

    ```bash
    spago build
    ```

### 编译、运行每个项目

每个文件夹都会指定运行 `spago` 命令来编译/运行程序。由于该文件夹的多项目性质，`spago` 命令必须在 `Hello World/Projects/` 中执行，而不是在每个项目的文件夹中。
要运行基于网络的程序，请按照文件夹的构建说明，然后打开 `dist/<项目名称>/<FP结构方法>/index.html`文件。
