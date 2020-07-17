# Types

类型系统定义如下：

- 原生类型: `Int`, `Number`, `String`, `Char`, `Boolean`
- Arrays
- Records
- Tagged Unions
- Newtypes
- Functions
- Polymorphic Types
- Constrained Types
- Type Synonyms
- Rows

## 基础类型

基础类型 `String`、`Numer` 和 `Boolean` 与 Javascript 一致。

```purescript

$ spago repl
> :type 1.0
Number

> :type "test"
String

> :type true
Boolean
```

### 整型数 Integer

`Int` 类型表示整数值,在运行时也表示为 JavaScript 中的`Number`。但是，对`Int`值上的操作（`+`）的定义不同，以确保始终获得`Int`类型的值。

```purescript

$ spago repl
> :type 1
Int
```

### 字符 Char

`Char` 类型

```purescipt

$ spago repl
> :type 'a'
Char
```

## 数组 Array

`Array`类型对应 Javascript 中的 `Array` 类型，但数组中所有元素都应是相同类型的。

```purescript

$ spago repl
> :type [1, 2, 3]
Array Int

> :type [1, false]
Could not match type Int with type Boolean.
```

## 记录 Record

记录对应 Javascript 中的 object。

```purescript

> author = { name: "Phil", interests: ["Functional Programming","JavaScript"] }

> :type author
{ interests :: Array String
， name :: String
}
```

## 标签联合 Tagged Union

Tagged Union 由一个或多个构造函数组成，每个构造函数都接受零个或多个参数。

Tagged Union 只能使用它们的构造函数创建，并通过模式匹配进行分解(稍后将提供更全面的模式匹配处理)。

举例：

```purescript
module Main where

import Prelude

import Effect (Effect)
import Effect.Console (log)

data Foo = Foo | Bar String

runFoo :: Foo -> String
runFoo Foo = "It's a Foo"
runFoo (Bar s) = "It's a Bar. The string is " <> s

main :: Effect Unit
main = do
  log (runFoo Foo)
  log (runFoo (Bar "Test"))
```

运行结果：

```bash
$ spago run
[info] Installation complete.
Compiling Main
[info] Build succeeded.
It's a Foo
It's a Bar. The string is Test
```

在这个例子中，`Foo` 是带有两个构造函数的 Tagged Union。第一个构造函数 `Foo` 没有参数，第二个 `Bar` 带有一个 `String` 类型的参数。

`runFoo` 是 Tagged Union 进行模式匹配来发现其构造函数的示例，最后两行显示了如何构造Foo类型的值。

## Newtypes

Newtypes 类似 `data` 类型（关键字 `data`引入），但仅限于包含单个参数的单个构造函数。 Newtypes 是通过 `newtype` 关键字引入的：

```purescript
newtype Percentage = Percentage Number
```

Newtypes 在运行时的表示形式与基础数据类型相同。 例如，类型 Percentage 的值在运行时只是一个 JavaScript 的数值。

Newtypes 可以分配自己的类型类(type class)实例，因此，例如，可以为 Percentage 赋予其自己的 Show 实例：

```purescript
instance showPercentage :: Show Percentage where
  show (Percentage n) = show n <> "%"
```

## 函数 Function

PureScript 中的函数就像它们的 JavaScript 的函数一样，但始终只有一个自变量。

## 多态类型 Polymorphic Type

表达式可以是多态类型：

```purescript
  identity x = x
```

`identity` 可以推断出具有（多态）类型 `forall t0. t0 -> to`。 这表示对于任何的 `t0`，`identity` 可以被赋予类型 `t0` 的值，并且将返回相同类型的值。

还可以添加类型注释：

```purescript
  identity :: forall a. a -> a
  identity x = x
```

## Row polymorphism

多态性(Polymorphism)不仅仅限于对类型的抽象。Value 可以通过多态的方式具有其他 kind，例如 rows 或者 effects （参考 [Kind 系统](https://github.com/purescript/documentation/blob/master/language/Types.md#kind-system)）。

例如，下面这个函数访问 record 中两个属性：

```purescript
  addProps o = o.foo + o.bar + 1
```

`addProps` 的类型可以推断出：

```purescript
  forall r. { foo :: Int, bar :: Int |r } -> Int
```

这里的类型变量 `r` 是 `Row Type` 类型。它表示 row type，可以用任何命名类型的 row 实例化它。

换句话说，`addProps` 接受具有 `foo` 和 `bar` 属性的 Record，以及任何其他 Record 属性。

因此，下面的代码也可以通过编译：

```purescript
  addProps { foo: 1, bar: 2, baz: 3 }
```

即使 `addProps` 的类型没有属性 `baz`。 但是，以下内容无法编译：

```purescript
 addProps { foo: 1 }
```

因为缺少 `bar` 属性。

## Rank N Types

`forall` 量化符可以出现在函数箭头的左侧、内部类型 Record 字段、数据构造器(data constructor)内以及类型同义词中。

在大多数情况下，使用这个特性时，需要进行类型注释(type annotation)。

举个例子，我们可以将一个多态函数作为参数传递给另一个函数。

```purescript
poly :: (forall a. a -> a) -> Boolean
poly f = (f 0 < 1) == f true
```

请注意，多态函数的类型参数被实例化为 `Number` 和 `Boolean`。

`poly` 的参数确实必须是多态的。例如，下面的代码就无法通过编译：

```purescript
test = poly (\n -> n + 1)
```

因为 `poly` 的类型签名中的 `forall` 所约束的类型变量就是刚性变量`a`并不与`Int`一致。

## Rows

Row 类型表示具有重复项的无序的命名类型集合。 重复标签的类型按顺序收集在一起，就像在`NonEmptyList` 中一样。 这意味着从概念上讲，Row 可以视为类型级别(type-level)的 `Map Label (NonEmptyList Type)`。

Row 不是一种类型(Type)：Row 对于某种 kind `k` 具有 kind `Row k`，因此行不能作为值存在。 相反，Row 可以在类型签名(type signatures)中用于定义记录类型或其他类型，在这些类型中，标签的无序类型是有用的。

要表示一个closed row 可以用逗号分隔字段，每个标签与其类型之间用双冒号分隔。

```purescript
( name :: String, age :: Numer)
```

要表示一个open row（例如一个可以与另一个 row 合并以添加新字段的行），用管道（`|`）将指定的术语与行变量分开。

```purescript
( name :: String, age :: Number | r )
```

## 类型同义 Type Synonyms

为了方便起见，可以使用 `type` 关键字声明类型的同义词。Type Synonyms 可以包含类型参数，但[不能partially applied](https://github.com/purescript/documentation/blob/master/errors/PartiallyAppliedSynonym.md#partiallyappliedsynonym-error)。Type Synonyms 可以与任何其他类型一起构建，但是[不能循环引用](https://github.com/purescript/documentation/blob/master/errors/CycleInTypeSynonym.md#cycleintypesynonym-error)。

例如：

```purescript
-- Create an alias for a record with two fields
type Foo = { foo :: Number, bar :: Number }

-- Add the two fields together, since they are Numbers
addFoo :: Foo -> Number
addFoo o = o.foo + o.bar

-- Create an alias for a polymorphic record with the same shape
type Bar a = { foo :: a, bar :: a }
-- Foo is now equivalent to Bar Number

-- Apply the fields of any Bar to a function
combineBar :: forall a b. (a -> a -> b) -> Bar a -> b
combineBar f o = f o.foo o.bar

-- Create an alias for a complex function type
type Baz = Number -> Number -> Bar Number

-- This function will take two arguments and return a record with double the value
mkDoubledFoo :: Baz
mkDoubledFoo foo bar = { foo: 2.0*foo, bar: 2.0*bar }

-- Build on our previous functions to double the values inside any Foo
-- (Remember that Bar Number is the same as Foo)
doubleFoo :: Foo -> Foo
doubleFoo = combineBar mkDoubledFoo

-- Define type synonyms to help write complex Effect rows
-- This will accept further Effects to be added to the row
type RandomConsoleEffects eff = ( random :: RANDOM, console :: CONSOLE | eff )
-- This limits the Effects to just RANDOM and CONSOLE
type RandomConsoleEffect = RandomConsoleEffects ()
```

与 `newtypes` 不同，Type Synonyms 只是别名，无法与它们的扩展用法区分开。因此，它们不能用于声明类型类实例。更多信息的请参考 [`TypeSynonymInstance` 错误](https://github.com/purescript/documentation/blob/master/errors/TypeSynonymInstance.md#typesynonyminstance-error)。

## 约束类型 Constrained Types

多态类型可以基于一个或多个 `constraints` 来确定。更多信息请参考 Type class 的章节。

## 类型注解 Type Annotations

大多数类型可以被推断出（不包括 Rank N Type 和 constrained type），但是可以在声明或在表达式之后使用双冒号 `::` 提供注释：

```purescript
-- Defined in Data.Semiring
one :: forall a. (Semiring a) => a

-- one can be an Int, since Int is an instance of Semiring where one = 1
int1 :: Int
int1 = one -- same as int1 = 1
-- Or even a Number, which also provides a Semiring instance where one = 1.0
number1 = one :: Number -- same as number1 = 1.0
-- Or its polymorphism can be kept, so it will work with any Semiring
-- (This is the default if no annotation is given)
semiring1 :: forall a. Semiring a => a
semiring1 = one
-- It can even be constrained by another type class
equal1 = one :: forall a. Semiring a => Eq a => a
```

## Kind System

Kind 类型是其他类型。约束(constraint)是唯一不支持kind的类型，没有 kind-level 的等价类型。多态 Kind 通过 `data`、`type`、`newtype` 和 `class` 声明的顶 top-level kind 签名实现的。

```purescript
-- Defined in Type.Proxy
data Proxy :: forall k. k -> Type
data Proxy a = Proxy

proxy1 = Proxy :: Proxy Int -- k is Type
proxy2 = Proxy :: Proxy "foo" -- k is Symbol
proxy3 = Proxy :: Proxy (foo :: Int, bar :: String) -- k is Row Type
```

与其他类型变量一样，kind 变量在类型签名中也要使用`forall`进行量化。 多态 kind 必须在引用它们的任何种类带注释变量之前进行量化：

```purescript
proxy :: forall k (a :: k). Proxy a
proxy = Proxy
```

## Quantification

类型变量(type variable)不仅可以是 type 或 row，还可以是 type constructor 或 row constructor 等，并且具有这些类型的类型变量都可以绑定在`forall`量词内。
