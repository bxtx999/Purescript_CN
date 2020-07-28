# PureScript 与 Haskell 的区别

## 评估策略

与 Haskell 不同，PureScript 采用严格的评估策略。

由于评估策略与 JavaScript 相匹配，因此与现有代码的互操作性是微不足道的-——从 PureScript 模块中导出的函数与任何“普通” JavaScript 函数的行为完全一样，相应地，通过 FFI 调用 JavaScript 也很简单。

保持严格的评估也意味着不需要运行时系统或过于复杂的 JavaScript 输出。在需要的时候也应该可以写出更高性能的代码，因为在 JavaScript 之上引入懒惰会带来不可避免的开销。

## Prelude/base

PureScript 中没有隐式导入 `Prelude` 包，`Prelude` 模块与其他模块一样。此外，编译器不分发任何库。

普遍接受的标准库 `Prelude` 是 [purescript-prelude](https://github.com/purescript/purescript-prelude) 库。

## 模块的导入与导出

使用关键字 `class` 导入模块中的类型类。

```purescript
module B where

import A (class Fab)
```

PureScript 中没有关键字 `qualified`。Hakell 中的 `import qualified Data.List as List` 与 PureScript 中的 `import Data.List as List` 等效。

更多关于模块导入和导出的内容请查看 [模块](language/Modules.md)。

## 类型

### 显式 forall

PureScript 中的多态函数在使用它们之前需要显式的 `forall`来声明类型变量。 例如，Haskell的列表长度函数声明如下：

```haskell
length :: [a] -> Int
```

但在 PureScript 中可能会报 `Type variable a is undefined` 的错误。 PureScript 的等效代码是：

```purescript
length :: forall a. Array a -> Int
```

`forall` 可以同时声明多个类型变量，并且应该出现在类型类约束之前：

```purescript
ap :: forall m a b. (Monad m) => m (a -> b) -> m a -> m b
```

### 数字类型

PureScript 中有能够表示 JavaScript 标准 IEEE 754 浮点数和 32 位整数的 `Int` 的原生 `Number` 类型。 在 JavaScript 中，`Int` 值和操作可以通过 `|0` 后缀表达。比如说，如果你有 `Int` 类型的 `x`、`y` 和 `z` 变量，PureScript 表达式 `(x + y) * z` 会被编译成 `((x + y)|0 * z)|0`。

### `Uint`

PureScript 中用 `Uint` 类型取代了 Haskell 中的 `()`。`Prelude` 模块提供 `uint` 类型。

### `[a]`

PureScript 没有为列表类型提供语法糖。使用 `Data.List` 中的 `List` 构造list 类型。

## Record

PureScript 可以通过使用 row types 直接编码成 JavaScript 风格的对象，所以定义在 PureScript 中 Haskell-style 的 Record 实际上有完全不同的含义。

```purescript
data Point = Point { x :: Number, y :: Number }
```

在 Haskell 中，这样的定义将给当前环境带来几件事：

```purescript
Point :: Number -> Number -> Point
x :: Point -> Number
y :: Point -> Number
```

```purescript
type PointRec = { x :: Number, y :: Number }
```

```purescript
data Point = Point { x :: Number, y :: Number }
```

```purescript
originX :: Number
originX  = origin.x
```

```purescript
setX :: Number -> PointRec -> PointRec 
setX val point = point { x = val }
```

```purescript
showPoint :: Point -> String
showPoint p = show p.x <> ", " <> show p.y
```

```purescript
showPoint :: Point -> String
showPoint (Point obj) = show obj.x <> ", " <> show obj.y
```

## 类型类

### 箭头方向

```purescript
class (Eq a) <= Ord a where
  ...
```

### 命名实例

```purescript
instance arbitraryUnit :: Arbitrary Unit where
  ...
```

### 派生

```purescript
data Foo = Foo Int String deriving (Eq, Ord)
```

```purescript
data Foo = Foo Int String

derive instance eqFoo :: Eq Foo
derive instance ordFoo :: Ord Foo
```

### 孤儿实例

### 默认成员

### 类型类分级

## 元组

## 合成算子

## `return`

## 列表推导

PureScript 没有为列表推导提供特殊的语法。

```purescript
import Prelude (($), (*), (==), bind, pure)
import Data.Array ((..))
import Data.Tuple (Tuple(..))
import Control.MonadZero (guard)

factors :: Int -> Array (Tuple Int Int)
factors n = do
  a <- 1 .. n
  b <- 1 .. a
  guard $ a * b == n
  pure $ Tuple a b
```

## `$` 的非特殊处理

## 定义算子

在 Haskell 中，可以用以下自然语法定义运算符

```haskell
f $ x = f x
```

在 PureScript 中，为命名函数提供运算符别名。 从 0.9 版开始，删除了使用运算符定义函数的功能。

```purescript
apply f x = f x
infixr 0 apply as $
```

## Operator Sections

在 Haskell 中，有一些语法糖可以 partially apply 中缀操作符。

```haskell
(2 ^) -- desugars to `(^) 2`, or `\x -> 2 ^ x`
(^ 2) -- desugars to `flip (^) 2`, or `\x -> x ^ 2`
```

在 PureScript 中，Operator Sections 看起来有点不同。

```purescript
(2 ^ _)
(_ ^ 2)
```

## 扩展

PureScript 编译器不支持类 GHC 的语言扩展。但是，有一些“内置”语言特性与许多 GHC 扩展等价(或至少类似)。这些目前都是：

- DataKinds (see note below)
- EmptyDataDecls
- ExplicitForAll
- FlexibleContexts
- FlexibleInstances
- FunctionalDependencies
- KindSignatures
- MultiParamTypeClasses
- PartialTypeSignatures
- RankNTypes
- RebindableSyntax
- ScopedTypeVariables

注意 `DataKinds`：不同于 Haskell，用户定义的类型是开放的，它们没有提权，这表示它们的构造函数只能在类型中使用，而不能在值中使用。浏览有关 [kind 系统](Types.md#kind-system)的更多信息。

## `error` 和 `undefined`

对于 `error`，你可以使用定义在 `purescript-exceptions` 包中的 `Effect.Exception.Unsafe.unsafeThrow`。

`undefined` 可以使用 `purescript-unsafe-coerce` 包中的 `Unsafe.Coerce.unsafeCoerce unit :: forall a. a` 模拟。更多信息可以查看 https://github.com/purescript/purescript-prelude/issues/44 。

不过请注意，由于 PureScript 的严格性，这些可能会与Haskell 版本有不同。

## 文档注释

编写文档时，管道字符 `|` 必须出现在每个注释行的开头，而不仅仅是第一行。有关更多详细信息，请参见文档以获取文档注释。

## Haskell 一些代码在哪里
