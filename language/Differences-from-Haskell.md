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

### 显示 forall

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

### `Uint`

### `[a]`

## Record

## 类型类

### 箭头方向

### 命名实例

### 派生

### 孤儿实例

### 默认成员

### 类型类分级

## 元组

## 合成算子

## `return`

## 列表推导

## `$` 的非特殊处理

## 定义算子

## 算子 section

## 扩展

## `error` 和 `undefined`

## 文档注释

## Haskell 一些代码在哪里
