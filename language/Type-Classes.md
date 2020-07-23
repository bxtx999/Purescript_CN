# 类型类

PureScript 通过 `class` 和 `instance` 关键字支持类型类(type classes)。

类实例中的类型必须是 `String`、`Number`、`Boolean` 或 `C t1 ... tn` 的形式，其中 `c` 是一个类型构造函数（包括 `->` 和 `t_i` 是相同的类型）。

这里是 `Show` 类型类的一个例子，实例的类型是 `String`、`Boolean` 和 `Array`：

```purescript
class Show a where
  show :: a -> String

instance showString :: Show String where
  show s = s

instance showBoolean :: Show Boolean where
  show true = "true"
  show false = "false"

instance showArray :: (Show a) => Show (Array a) where
  show xs = "[" <> joinWith ", " (map show xs) <> "]"

example = show [true, false]
```

PureScript 中不再允许重叠实例。要编写重叠的实例，您应该使用实例链（Instance Chains）。

## 实例链

PureScript 实现了一种实例链的形式，它工作在由参数匹配的实例组上。这意味着在选择实例时不考虑约束。然而，您仍然可以使用 `else` 关键字按连续的顺序编写一个实例链，该实例链将从上到下进行匹配。

这里是一个 `MyShow` 类型类的例子，有`String`、`Boolean` 和任何其他类型的实例。

```purescript
class MyShow a where
  myShow :: a -> String

instance showString :: MyShow String where
  myShow s = s

else instance showBoolean :: MyShow Boolean where
  myShow true = "true"
  myShow false = "false"

else instance showA :: MyShow a where
  myShow _ = "Invalid"

data MysteryItem = MysteryItem

main = do
  log $ myShow "hello" -- hello
  log $ myShow true -- true
  log $ myShow MysteryItem -- Invalid
```

## 多参数类型类

更多信息查看：[PureScript by Example](https://book.purescript.org/chapter6.html#multi-parameter-type-classes)

## 超级类

超级类的含义可以在类声明中用一个向后的箭头 `<=` 表示。

```purescript
class (Monad m) <= MonadFail m where
  fail :: forall a. String -> m a
```

这个代码示例定义了一个带有 `Monad` 超级类的 `MonadFail` 类：任何定义了 `MonadFail` 实例的类型也需要定义一个 `Monad` 的实例。

当搜索一个子类的实例时，将使用超级类实例。例如，在下面的代码中，纯函数引入的 `Applicative` 约束可以被解除，因为`Applicative` 是 `Monad` 的超类，而 `Monad` 又是 `MonadFail` 的超类。

```purescript
assert :: forall m. (MonadFail m) => Boolean -> m Unit
assert true = pure unit
assert false = fail "Assertion failed"
```

## 孤儿实例

在定义该类的模块和定义该类型的模块之外定义的类型类实例称为*孤儿实例(orphan instance)*。一些编程语言（包括 Haskell）允许孤儿实例，不过会发出警告，但在 PureScript 中孤儿实例是被禁止的。在 PureScript 中定义孤例的任何尝试都意味着您的程序不能通过类型检查。

例如，`Semigroup` 类型类被定义在 `Data.Semigroup` 模块中，而 `Int` 类型被定义在 `Prim` 模块中。如果我们试图这样定义一个`Semigroup Int` 实例：

```purescript
module MyModule where

import Prelude

instance semigroupInt :: Semigroup Int where
  append = (+)
```

代码将编译失败，因为 `semigroupInt` 是一个孤儿实例。您可以使用 `newtype` 来解决此问题：

```purescript
module MyModule where

import Prelude

newtype AddInt = AddInt Int

instance semigroupAddInt :: Semigroup AddInt where
  append (AddInt x) (AddInt y) = AddInt (x + y)
```

事实上，在 `monoid` 包中的 `Data.Monoid.Additive` 中提供了一个类似于这个 `AddInt` 的类型。

孤儿实例是被禁止的，因为它们会导致同一类型和类的不兼容的重复实例。举个例子，假设两个独立的模块定义了一个孤例的 `Semigroup Int` 实例，其中一个使用 `+` 来进行 `append`，而另一个则使用 `*`。现在假设有人写了第三个模块，它同时导入了前两个模块，并且在第三个模块中的某个地方有表达式 `2 <> 3`，它调用了一个 `Semigroup Int` 实例。编译器现在有两个实例可供选择。它应该怎么做呢？它可以报告一个错误，或者它可以任意选择其中一个实例。这两种选择都不是特别好。

- 如果它选择报错，就意味着任何一对定义了相同孤例的模块 都不能一起使用。
- 如果它任意选择一个，我们将无法确定 `2 <>3` 将评估为 `5` 还是 `6`。这可能会使我们很难保证你的程序能够正确地运行!

禁止孤儿实例也保证了实例的全局唯一性。如果没有全局唯一性，您就有可能在不同代码部分用不兼容的实例来操作数据。例如，在基于 `Ord` 的地图和集合中，如果可以使用一个 `Ord` 实例将一些值插入到地图中，然后尝试使用不同的 `Ord` 实例来检索它们，你就会有键从你的地图中消失。另一个例子是，如果你有一个定义了序列化和反序列化操作的类型类，你就可以用一个实例序列化一些东西，然后尝试用不同的不兼容的实例来反序列化它。

对于多参数类型类，孤儿实例检查要求实例与类在同一模块，或者与实例中发生的至少一个类型在同一模块。(TODO: example)

## 功能依赖

对于有多个参数的类型类的实例，一般只需要将参数的一个子集具体化来匹配实例。实例头中关于哪些参数可以决定其他参数的声明称为功能依赖。

例如：

```purescript
class TypeEquals a b | a -> b, b -> a where
  to :: a -> b
  from :: b -> a

instance refl :: TypeEquals a a where
  to a = a
  from a = a
```

符号 `|` 标志着功能依赖的开始，如果有多个功能依赖，则用逗号分隔。在这种情况下，第一个参数决定第二个参数的类型，第二个参数决定第一个参数的类型。

功能依赖性对于各种 `Prim` 类型类特别有用，例如 `Prim.Row.Cons`：https://pursuit.purescript.org/builtins/docs/Prim.Row#t:Cons

也请参见[PureScript by Example 的章节](https://book.purescript.org/chapter6.html#functional-dependencies)。

## 类型类派生

一些类型类实例可以通过 PureScript 编译器自动派生。 可以通过 `derive instance` 关键字来派生类型类实例。

```purescript
newtype Person = Person { name :: String, age :: Int }

derive instance eqPerson :: Eq Person
derive instance ordPerson :: Ord Person
```

当前，编译器可以派生以下类型类：

- [Data.Generic.Rep (class Generic)](https://pursuit.purescript.org/packages/purescript-generics-rep/docs/Data.Generic.Rep#t:Generic)
- [Data.Eq (class Eq)](https://pursuit.purescript.org/packages/purescript-prelude/docs/Data.Eq#t:Eq)
- [Data.Ord (class Ord)](https://pursuit.purescript.org/packages/purescript-prelude/docs/Data.Ord#t:Ord)
- [Data.Functor (class Functor)](https://pursuit.purescript.org/packages/purescript-prelude/docs/Data.Functor#t:Functor)
- [Data.Newtype (class Newtype)](https://pursuit.purescript.org/packages/purescript-newtype/docs/Data.Newtype#t:Newtype)

请注意，`derive instance` 并不是让你避免写出模板型类实例代码的唯一机制。这里没有列出其他方式派生类型类的其他方法，例如通过 `Generic实例`。例如，下面是如何通过 `genericShow` 为 `Person` 创建一个 `Show` 实例：

```purescript
import Data.Generic.Rep (class Generic)
import Data.Generic.Rep.Show (genericShow)

derive instance genericPerson :: Generic Person _

instance showPerson :: Show Person where
  show = genericShow
```

更多有关  Generic deriving 的信息，请参考 [generics-rep 库文档](https://pursuit.purescript.org/packages/purescript-generics-rep)。

## 编译器可解决的类型类

有些类型类可以由 PureScript 编译器自动解决，而不需要您在源代码中的 PureScript 语句，如 `derive instance`。

```purescript
foo :: forall t. (Warn "Custom warning message") => t -> t
foo x = x
```

自动解决的类型类在 `Prim` 模块中：

Symbol-related classes

- [`IsSymbol`](https://pursuit.purescript.org/packages/purescript-symbols/3.0.0/docs/Data.Symbol#t:IsSymbol)
- [`Append`](https://pursuit.purescript.org/builtins/docs/Prim.Symbol#t:Append)
- [`Compare`](https://pursuit.purescript.org/builtins/docs/Prim.Symbol#t:Compare)
- [`Cons`](https://pursuit.purescript.org/builtins/docs/Prim.Symbol#t:Cons)

[Prim.Row](https://pursuit.purescript.org/builtins/docs/Prim.Row)

- [`Cons`](https://pursuit.purescript.org/builtins/docs/Prim.Row#t:Cons)
- [`Union`](https://pursuit.purescript.org/builtins/docs/Prim.Row#t:Union)
- [`Nub`](https://pursuit.purescript.org/builtins/docs/Prim.Row#t:Nub)
- [`Lacks`](https://pursuit.purescript.org/builtins/docs/Prim.Row#t:Lacks)

[Prim.RowList](https://pursuit.purescript.org/builtins/docs/Prim.RowList)

- [`RowToList`](https://pursuit.purescript.org/builtins/docs/Prim.RowList#t:RowToList)

Other classes

- [`Partial`](https://pursuit.purescript.org/builtins/docs/Prim#t:Partial)
- [`Fail`](https://pursuit.purescript.org/builtins/docs/Prim.TypeError#t:Fail)
- [`Warn`](https://pursuit.purescript.org/builtins/docs/Prim.TypeError#t:Warn)