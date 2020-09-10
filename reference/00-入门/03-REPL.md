# REPL

`REPL` 表示 Read、Evaluate、Print 和 Loop。

## 启动 REPL

使用 `spago repl`

```purescript
❯ spago repl
[info] Initializing a sample project or migrating an existing one..

...

PSCi, version 0.13.8
Type :? for help

import Prelude

>
```

## 使用 REPL

通常，在 REPL 中你可以做以下五件事情：

1. 查看输入到 REPL 中表达式（例如 `3 + 3`）的结果，单击 `Enter` 键。

2. 使用语法 `binding = <expression>` 定义变量或函数的绑定。例如：

   - `x = 3`

   - `function = (\x -> x + 1)`

3. 使用 `:paste` 命令输入多行表达式（`CTRL+D` 结束输入）。

4. 使用其他命令来研究模块的函数、类型和 Kind。

5. 使用其他命令与 REPL 的当前状态进行交互。（例如，清除绑定和/或导入的模块，显示已导入的模块等）

## REPL 可能输出错误

有时，REPL 会输出错误。这些错误对于新的学习者来说可能无法立即理解，所以下面的表格将帮助你理解这些错误并知道该怎么做。

| 错误 | 含义 | 如何解决 |
| - | - | - |
| "No type class instance was found for `Data.Show.Show [Type]`" | An expression cannot be turned into a `String`. For example, a function's implementation (`(\x -> x + 1)`) cannot be turned into a `String` whereas a value (`5`) or expression (`10 + 10`) can be (`5` and `20`, respectively). | If it's possible for you to define one, define an instance of the `Show` type class. If not, then ignore it and move on.
| "Multiple value declarations exist for [binding]." | You defined the binding twice, which you cannot do | See [the Reload command section](#reload) for what your options are |

## REPL命令的快速概述

REPL 提供了一些命令。您可以通过键入 `:help` 或 `:?` 并按 `Enter` 键来查看整个列表。

这些命令与 `:?` 的输出顺序相同。

注意: 这些命令可以缩短为它们的第一个字母。因此，与其输入 `:type`，不如输入 `:t`。同样，与其输入 `:paste` 或 `:print`，不如分别输入 `:pa` 或 `:pr`。

### Help

通过 `:help` 或 `:?` 获取 REPL 命令。

### Quit

退出REPL，并返回给shell。

### Reload

#### 问题

只能定义一次绑定。用不同的表达式再次定义它将会输出一个错误：

```purescript
x = 5 -- first time
x = 6 -- second time raises error
-- REPL's outputs error: "Multiple value declarations exist for x."
```

您需要清除 `x` 的绑定，才能将其重新用于其他绑定。

例如，假设您编写了两个函数，第二个使用第一个。 但是，您第二次编写了错误的实现，需要重写它：

```purescript
add1 = (\x -> x + 1)
times2 = (\x -> x * 3) -- "3" should be "2"
```

#### 解决方法

理想情况下，您可以清除第二个函数的绑定并重写它。

不幸的是，你不能这样做。你可以：

1. 使用 `:reload` 命令清除两个函数的绑定，重新定义第一个函数，然后用正确的实现定义第二个函数。

1. 为正确的实现定义新的绑定。

    ```purescript
    -- 1st option
    add1 = (\x -> x + 1)
    times2 = (\x -> x * 3) -- Whoops! "3" should be "2"
    :reload
    add1 = (\x -> x + 1) -- define the "add1" binding again
    times2 = (\x -> x * 2) -- define "times2" again but with correct implmentation.

    -- 2nd option
    add1 = (\x -> x + 1)
    times2 = (\x -> x * 3) -- Whoops! "3" should be "2"
    times2_fix = (\x -> x * 2) -- define new function with correct implementation
    ```

1. 在文件中定义您的代码 (作为一个模块) 并将该模块导入您的 REPL 会话中。任何对该文件的编辑都会在 REPL 重新加载时被接收。

    创建一个包含REPL脚本的文件：

    ```purescript
    -- MyModule.file
    module MyModule where

    import Prelude

    add1 = (\x -> x + 1)
    times2 = (\x -> x * 3) -- This typo will be fixed later
    ```

    将脚本加载到 REPL 中：

    ```purescript
    > import MyModule
    > times2 4
    12
    ```

    对这个文件进行任何编辑。例如，改成 `times2 = (\x -> x * 2)`。保存文件，然后在现有的 REPL 会话中重新加载 `MyModule` 模块。

    ```purescript
    > :reload
    > times2 4
    8
    ```

### Clear

使用 :cl 而不是 :c 来区分这个命令和 :complete。这与 :reload 的工作原理相同，只是所有导入的模块也会被删除。如果您这样做，您将需要重新导入任何您希望使用的模块。例如，您可能需要重新导入 Prelude（导入 Prelude），这样您就可以再次使用数字运算（即 +, -, /, *）和 == 函数。

### Browse

查看模块导出并可以使用的所有函数、类型和类型类。

### Type

显示了一个值、一个函数或一个类型类的类型签名。能够根据类型签名来确定函数的主体是做什么的，所以不显示该主体。

```purescript
> :type x + 1
Int
> :type (\x -> x + 1)
Int -> Int
```

### Kind

显示类型的 kind。 Kind 将会在语法中详细解释。

```purescript
> :kind Int
Type
> :kind (Int -> Int)
Type
> :kind Array
Type -> Type
```

### Show

其中有两个命令:

- `show loaded`/`:s loaded` 显示 REPL 会话所知道的所有模块。有些模块可能已经导入，也可能尚未导入。(在 REPL 会话开始之前，PureScript 编译器会根据给它的源 globs 编译所有 PureScript 文件。这些 globs 中的所有模块都是 REPL 会话所知道的， 但您可能不希望在给定的会话中使用所有模块。)
- `show import`/`:s import` 显示您当前REPL会话中已导入的模块

### Print

改变表达式被 evaluate 后如何将值打印到控制台。默认情况下，它使用 `PSCI.Support.eval`。

新手可以暂时忽略这个命令。熟悉语言的人可以通过调用 `:print Path.To.Module.functionName` 将其改为其他命令。

无论如何都可以调用 `:print PSCI.Support.eval` 将它重置为默认值。

### Paste

REPL 只接受单行 Purescript 代码。如果有任何需要您编写多行表达式的内容，您必须使用 `:paste` 命令。

工作流程是这样的:

1. 输入粘贴命令：`:paste`

1. 步骤如下：

   - 输入多行表达式（例如，类型类及其函数、数据类型及其值、函数的类型签名及其实现，等等。）

   - 将一些外部代码粘贴到 REPL 中

1. 按 `CMD+D` 或 `CTRL+D` 完成输入。

然后，REPL 将解析所有代码，让您能够从那时起使用。

### Complete

REPL 已经支持制表符补全。 因此，此命令并不意味着用户可以使用。 相反，它用于需要一种方式来完成制表符补全的工具。 有关上下文，请参见[Harry的评论](https://github.com/purescript/purescript/issues/3746#issuecomment-550512591)。
