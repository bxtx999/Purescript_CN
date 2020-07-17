# PureScript 入门

让我们来了解一下使用 PureScript 编译器`purs` 和它的交互模式 `purs repl` 的基本设置。

我们将从安装编译器和Spago构建工具开始，然后讲解`purs repl`的基本用法，并解决[Project Euler](http://projecteuler.net/problem=1)问题。

#### 安装编译器

首先需要安装 [Node.js 和 NPM](https://docs.npmjs.com/getting-started/installing-node) 。可能您还需要安装 Haskell 的 [Stack](https://docs.haskellstack.org/en/stable/README/) 工具。

使用 `npm` 安装 PureScript 的编译工具 `purs`:

```bash
npm install -g purescript
```

运行 PureScript 编译器确保可执行文件已经保存在 `$PATH` 中。

```bash
purs
```
 
也可以从 [Hackage](http://hackage.haskell.org/package/purescript) 或通过下载最新的 [binary bundle](https://github.com/purescript/purescript/releases) 安装。 您的操作系统。 如果这样做，请确保 `purs` 可执行文件位于 `$PATH` 中。

#### 设置开发环境

推荐使用 [Spago](https://github.com/spacchetti/spag) 作为 PureScript 的包管理工具和构建工具。

安装 Spago：

```bash
npm install -g spago
```

使用 `spago init` 创建空项目：

```bash
mkdir my-project
cd my-project
spago init
```

当前目录包含以下文件：

- `packages.dhall` —— 包含 Spago 的配置
- `spago.dhall` —— 包含库文件的依赖信息
- `src/Main.purs` —— 项目的入口
- `test/Main.purs` —— 测试代码

可以通过以下命令构建并测试项目：

```bash
spago build
spago test
```

接下来可以看到以下输出：

```bash
[info] Installation complete.
[info] Build succeeded.
🍝
You should add some tests.
[info] Tests succeeded.
```

如果项目构建成功并且测试没有问题，那么在最后一行将会输出 "Test suceeded."。

#### 安装依赖

我们可以使用 Spago 来安装依赖文件。以安装 `lists` 库为例：

```bash
spago install lists
```

#### PSCI模式

PSCi 是 PureScript 的交互模式，对于处理纯计算和测试思想很有用。

在命令行中使用 `spago repl` 打开 PSCi 模式。也可以手动创建 `.purs-repl` 文件，在 PSCi 中加载模块和依赖。如果直接调用 PSCi 可执行文件，则需要手动加载这些文件。

```
PSCi, version 0.12.0
Type :? for help

import Prelude

> 
```

可以通过 `:?` 查看命令列表：

```bash
    The following commands are available:

    :?                        Show this help menu
    :quit                     Quit PSCi
    :reload                   Reload all imported modules while discarding bindings
    :clear                    Discard all imported modules and declared bindings
    :browse      <module>     See all functions in <module>
    :type        <expr>       Show the type of <expr>
    :kind        <type>       Show the kind of <type>
    :show        import       Show all imported modules
    :show        loaded       Show all loaded modules
    :paste       paste        Enter multiple lines, terminated by ^D
    :complete    <prefix>     Show completions for <prefix> as if pressing tab

    Further information is available on the PureScript documentation repository:
    --> https://github.com/purescript/documentation/blob/master/guides/PSCi.md
```

在本教程中，我们将选择这些命令。

首先按 Tab 键以使用自动完成功能。 您将看到 Prelude 中可用的功能名称集合。

要查看这些值的类型，请首先使用 `import` 命令导入相应的模块。

接下来，使用 `:type` 命令，后跟一个空格，然后是值的名称：

```bash

> import Prelude
> :type map
forall a b f. Functor f => (a -> b) -> f a -> f b

> import Data.List
> :type zip
forall a b. List a -> List b -> List (Tuple a b)

```

我们将使用 Prelude 和 Data.List 模块中的某些功能，因此请确保已使用 `import` 关键字导入了这些功能：

```bash
import Prelude
import Data.List
```

注意，使用 "Tab" 自动完成功能可以在 " psci" 中节省时间。

#### 解决 Euler 问题 #1

下面的问题来自于[Project Euler](http://projecteuler.net/problem=1)。

> If we list all the natural numbers below 10 that are multiples of 3 or 5, we get 3, 5, 6 and 9. The sum of these multiples is 23.
>
> Find the sum of all the multiples of 3 or 5 below 1000.

我们可以在 REPL 中利用函数解决问题。

首先，利用 `Data.List` 中的 `range` 创建包含 1000 以下自然数列表。

```bash
> range 0 999
```

您应该看到一个在命令行上打印了1000个元素的列表。

设置变量名：

```bash
> ns = range 0 999
```

现在，让我们过滤掉所有不符合条件的元素。 我们可以通过提供谓词函数作为Data.List中的filter函数的第一个参数来使用：

```bash
> multiples = filter (\n -> mod n 3 == 0 || mod n 5 == 0) ns
```

您可以查看 `multiples` 的值或检查它的属性。

```bash
> multiples
(0 : 3 : 5 : 6 : ...

> :type multiples
List Int
```

现在我们需要计算 `multiples` 列表的和。 我们可以使用 `Data.Foldable` 模块中的 `sum` 函数。

```bash
> import Data.Foldable
> sum multiples
233168
```

使用 `:quit` 退出 PSCi。

```bash
> :quit
See ya!
```

#### 编译代码

现在，我们已经了解了如何使用 REPL 来解决问题。现在让我们将解决方案移到源文件并进行编译。

创建一个新的文本文件 `src/Euler.purs`并复制以下代码：

```purescript
module Euler where

import Prelude

import Data.List (range, filter)
import Data.Foldable (sum)

ns = range 0 999

multiples = filter (\n -> mod n 3 == 0 || mod n 5 == 0) ns

answer = sum multiples
```

这些代码阐述了有关模块的一些关键思想：

- 每个文件都以模块头开头。模块名称由一个或多个用点分隔的大写单词组成。在这种情况下，仅使用单个单词，但是 `My.First.Module` 将是同等有效的模块名称。

- 使用模块的全名导入模块，包括用于分隔模块名称各部分的点。 在这里，我们导入 `Prelude` 模块，该模块提供 `mod`，`==` 和许多其他常用功能。 我们还导入了`Data.List`，它提供了明确列出的 `range` 和 `filter` 函数。 我们可以像使用 `Prelude` 一样隐式地将所有函数导入模块中，也可以显式列出它们。指导原则是只有一个模块具有隐式导入。

可以将此文件直接加载到 REPL 中并继续执行：

```bash
spago repl
> import Euler
> answer
233168
> :quit
See ya!
```
我们也可以用 Spago 将模块编译成 JavaScript：

```bash
spago build
```

这会将存在于 `src/` 中的每个模块编译到位于 `output/` 目录中的单独文件中。

编译器将显示一些有关缺少类型声明的警告。通常，提供显式类型签名被认为是一种好习惯。在本指南中，为简洁起见，省略了它们。在没有类型签名的情况下，PureScript 编译器会自动推断类型，但会提醒我们考虑添加它们。

#### 编写测试

为了测试代码，我们要使用 `assert` 库：

```bash
spago install assert
```

修改 `test/Main.purs` 文件，并添加以下代码：

```purescript
module Test.Main where

import Prelude

import Euler (answer)
import Test.Assert (assert)

main = do
  assert (answer == 233168)
```

我们的测试代码仅仅判断 `answer` 的值是否正确。在真实的测试中，我们可能需要在 `main`函数中利用 `Effect` monad 组合更多的测试。

运行 `spago test` 进行测试，在最后一行应该可以看到 "Test OK"。

#### 创建可执行文件

我们可以在 `src/Main.purs`模块中修改 `main` 函数，以将结果打印到控制台：

```purescript
module Main where

import Prelude

import Euler (answer)
import Effect.Console (log)

main = do
  log ("The answer is " <> show answer)
```

使用 `spago run` 命令编译，并运行 `Main` 模块：

```bash
$spago run
[info] Build succeeded.
The answer is 233168
```

#### 编译浏览器可执行代码

我们可以使用 `spago bundle-app` 命令将 PureScript 代码转换为适合在 Web 浏览器中使用的 JavaScript 代码：

```bash
$ spago bundle-app
...
Build succeeded.
Bundle succeeded and output file to index.js
```

`src` 目录中的所有代码以及项目依赖项均已编译为 JavaScript。产生的代码被打包为 "index.js"，并且还删除了所有未使用的代码，这一过程称为无效代码消除。这个 `index.js` 文件现在可以包含在 HTML 文档中。如果您尝试这样做，则应该可以再浏览器控制台中看到 "Hello，World！"。 

如果打开 `index.js`，应该会看到一些编译后的模块，如下所示：

```javascript
// Generated by purs bundle 0.13.6
var PS = {};

// ...

(function($PS) {
  "use strict";
  $PS["Euler"] = $PS["Euler"] || {};
  var exports = $PS["Euler"];
  var Data_EuclideanRing = $PS["Data.EuclideanRing"];
  var Data_Foldable = $PS["Data.Foldable"];
  var Data_List = $PS["Data.List"];
  var Data_List_Types = $PS["Data.List.Types"];
  var Data_Semiring = $PS["Data.Semiring"];
  var ns = Data_List.range(0)(999);
  var multiples = Data_List.filter(function (n) {
      return Data_EuclideanRing.mod(Data_EuclideanRing.euclideanRingInt)(n)(3) === 0 || Data_EuclideanRing.mod(Data_EuclideanRing.euclideanRingInt)(n)(5) === 0;
  })(ns);
  var answer = Data_Foldable.sum(Data_List_Types.foldableList)(Data_Semiring.semiringInt)(multiples);
  exports["answer"] = answer;
})(PS);

(function($PS) {
  // Generated by purs version 0.13.6
  "use strict";
  $PS["Main"] = $PS["Main"] || {};
  var exports = $PS["Main"];
  var Data_Show = $PS["Data.Show"];
  var Effect_Console = $PS["Effect.Console"];
  var Euler = $PS["Euler"];
  var main = Effect_Console.log("The answer is " + Data_Show.show(Data_Show.showInt)(Euler.answer));
  exports["main"] = main;
})(PS);

PS["Main"].main();
```

这说明了有关PureScript编译器生成JavaScript代码的方式的几点：

- 每个模块都变成由包装函数创建的对象，该对象包含模块的导出成员。

- PureScript尝试尽可能保留变量名。

- PureScript中的函数应用程序变成了JavaScript中的函数应用程序。

- main方法在定义了所有模块之后运行，并作为不带参数的简单方法调用生成。

- PureScript代码不依赖任何运行时库。 编译器生成的所有代码都起源于您的代码所依赖的PureScript模块。

这些要点很重要，因为它们意味着 PureScript 生成简单易懂的代码。通常，代码生成过程是一个很浅的转换。只需很少的语言理解即可预测将为特定输入生成的 JavaScript 代码。

### 编译成 CommonJS 模块

Spago 还可以将 PureScript 代码生成 CommonJS 模块。这在使用 NodeJS 或仅在开发使用 CommonJS 模块将代码分解为较小组件的较大项目时很有用。

使用 `spago build` 命令构建 CommonsJS 模块：

```bash
$ spago build
...
Build succeeded.
```

默认情况下，生成的模块将放置在 "output" 目录中。每个 PureScript 模块将在自己的子目录中编译为自己的 CommonJS 模块。

要构建CommonJS模块，请使用`spago build`命令：

#### 下一步？

如果您不熟悉类型化的函数式编程，那么下一站应该是[PureScript示例](https://book.purescript.org/)，它将通过解决实际问题引导您学习PureScript。

如果您已经熟悉Haskell或Elm之类的ML系列语言，那么PureScript by Example应该仍然适合作为起点，但您也可以通过浏览[文档库中的语言参考](https://github.com/purescript/documentation/tree/master/language)。 语言参考提供了对该语言的更简短的参考样式描述，针对的是那些已经熟悉类型化函数编程的人。 还有一个[Haskell的区别](https://github.com/purescript/documentation/blob/master/language/Differences-from-Haskell.md)页面，Haskell程序员会发现这很有用。

还鼓励新的PureScript程序员花一些时间浏览[Pursuit](https://pursuit.purescript.org)，该网站托管了PureScript库的生成的API文档。 特别值得熟悉[核心库](https://github.com/purescript)（即那些托管在GitHub上`purescript`组织下的库），尤其是[prelude](https://pursuit.purescript.org/packages/purescript-prelude)，因为它们提供了许多基本概念，这些概念对于编写程序经常有用。