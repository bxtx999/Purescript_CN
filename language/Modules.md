# 模块

PureScript 中的所有代码都包含在模块中。 使用 `module` 关键字引入模块：

```purescript
module A where
  
id x = x
```

## 引入模块

可以使用 `import` 关键字导入模块,这称为“开放式导入”。它将为导入模块中的所有值和类型创建别名：

```purescript
module B where
  
import A
```

或者，可以在括号中提供要导入的名称列表：

```purescript
module B where
  
import A (runFoo)
```

值、操作符（用括号括起来）、类型构造函数、数据构造函数和类型类都可以显式导入。一个类型构造函数后面可以跟一个括号中的相关数据构造函数列表来导入。双点(`..`)可以用来导入一个给定类型构造函数的所有数据构造函数。

```purescript
module B where

import A (runFoo, (.~), Foo(..), Bar(Bar))
```

类型类用 `class` 关键字导入，种类用 `kind` 导入。

```purescript
module B where

import A (class Fab, kind Effect)
```

### 隐式导入

也可以使用 `hiding` 关键字从开放导入中排除某些名称。 这对于避免模块之间的导入冲突很有用：

```purescript
module C where
  
import A hiding (runFoo)
import B (runFoo)
```

## 限定导入

模块也可以有条件的导入，这意味着它们的名字不会被直接带入范围，而是被别名为不同的模块名。

以下是一些有条件的导入非常有用的情况：

### 使用通用函数

```purescript
import Data.Map as Map

a :: Map Int String
a = Map.fromFoldable [ Tuple 1 "a" ]
```

几个数据结构模块都有 `fromFoldable` 函数，可以用来从任何其他 `Foldable` 数据结构中创建该数据结构的实例。为了明确使用的是哪个 `fromFoldabl` e函数，我们可以将该模块的函数导入一个限定名下，并使用它限定，比如 `Set.fromFoldable`。

再举一个例子，这次用的是一个虚构的模块：

```purescript
import MyWebFramework as MyWebFramework

main :: Effect Unit
main = do
  elem <- domElementById "appContainer"
  MyWebFramework.run elem
  -- ^ this may be more clear than
  -- `run elem`
```

因为 "run" 是一个相当不具象的名字，如果在阅读它之前不知道run函数的类型，那么在我们看到它的模块是 `MyWebFramework` 之前，并不清楚期望 `run` 函数做什么。为了减轻新读者对这段代码的困惑，我们可以导入并限定调用它 —— `MyWebFramework.run`。

### 避免命名冲突

```purescript
module Main where

import Data.Array as Array

null = ...

test = Array.null [1, 2, 3]
```

在这里，`null` 这个名字通常会与 `Data.Array` 中的 `null` 发生冲突，但限定导入解决了这个问题。`Data.Array.null` 可以用 `Array.null` 代替引用。

操作符也可以用这种方式引用。

```purescript
test' = Array.null ([1, 2, 3] Array.\\ [1, 2, 3])
```

### 合并模块

模块可以使用限定导入的方式合并在同一名称下，如果合并多个模块，可以考虑使用显式导入的方式，避免冲突。如果要合并多个模块，可以考虑使用显式导入来避免冲突，以防模块想导入相同的名称。

```purescript
module Main where

import Data.String.Regex (split) as Re
import Data.String.Regex.Flags (global) as Re
import Data.String.Regex.Unsafe (unsafeRegex) as Re

split = Re.split (Re.unsafeRegex "[,;:.]\\s+" Re.global)
```

## 模块导出

您可以通过使用导出列表来控制从模块中导出的内容，当使用导出列表时，其他模块只能看到导出列表中的内容。当使用导出列表时，其他模块只能看到导出列表中的内容。比如说：

```purescript
module A (exported) where

exported :: Int -> Int
exported = [...]

notExported :: Int -> Int
notExported = [...]
```

在这种情况下，导入 `A` 的模块将无法看到 `notExported` 功能。

可以导出的名称类型与模块导入相同。

导入的模块可以全部重新导出：

```purescript
module A (module B) where

import B
```

还可以使用限定的和显式的导入：

```purescript
module A (module MoreExports) where

import A.Util (useful, usefulFn) as MoreExports
import A.Type (ADatatype(..)) as MoreExports
```

当重新导出其他模块时，也可以通过指定模块本身为导出对象来导出所有本地值和类型：

```purescript
module A (module A, module B) where

import B

data ...
```

### 导出类型类

要导出一个类型类，只需将其与所有成员一起添加到列表中即可。可惜的是，没有捷径可以一次性导出所有类型类成员。

例如，假设我们有以下内容：

```purescript
class Foldable f where
  foldr :: forall a b. (a -> b -> b) -> b -> (f a) -> b
  foldl :: forall a b. (b -> a -> b) -> b -> (f a) -> b
  foldMap :: forall a m. (Monoid m) => (a -> m) -> (f a) -> m
```

然后您可以导出类：

```purescript
module Test (class Foldable, foldr, foldl, foldMap) where
```

如果一个类型类被导出，那么它的所有成员也必须被导出。同样，如果一个类型类成员被导出，它所属的类型类也必须被导出。
