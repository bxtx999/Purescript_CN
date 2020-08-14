# 语法

这个文件夹使用元语言（一种描述语法的语言）来描述可编译 Purescript 语法。

因此，与其说是像下面的代码什么都没告诉你。

```purescript
f :: String -> Int
```

而元语言会告诉你

```purescript
functionName :: ParameterType -> ReturnType
```

由于语法可以被编译，所以可以验证它是有效和正确的语法。

因此，大多数文件会出现这样的情况：

```purescript
-- The module will be declared at the top of the file
--   It can be ignored.
module Syntax.ModuleName where

-- The Prelude module might be imported
--   It, too, can be ignored.
import Prelude

-- The thing that the file is documenting usually goes here.
--    Don't ignore this stuff.
data Box a = Box a

-- Sometimes the comment "necessary to compile" will appear.
-- It makes the meta-language compileable. Ignore everything underneath it
--   as you read through the files.

-- necessary to compile
type SomeTypeName = String
```

如果你想试验语法，可以参考下面的说明：

1. 转到一个有 `spago.dhall` 文件的目录下 (否则，这些命令的其他部分将无法使用)

1. 安装依赖项：`spago install`

1. 启动 REPL 或用监视建立文件（参考下表）

    | 命令 | 理想用法 | 其他注释
    | - | - | - |
    | `spago repl` | 小于10行代码 | 编辑 `.purs-repl` 并添加 `import ModuleName`，这样就可以在您运行此命令时自动导入该模块
    | `spago build --watch` | 测试10+行的语法 | 运行此命令后保存文件，将重新编译项目 |
