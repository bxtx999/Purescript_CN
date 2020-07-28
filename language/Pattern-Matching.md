# 模式匹配

模式匹配对一个值进行解构，将零个或多个表达式纳入作用域。模式匹配是用 `case` 关键字引入的。

模式匹配的一般形式如下：

```purescript
case value of
  pattern -> result
  ...
  pattern -> result
```

模式匹配也可以在函数声明中使用：

```purescript
fn pattern_1 ... pattern_n = result
```

在引入函数时也可以使用模式。例如：

```purescript
example x y z = x * y + z
```

以下形式可以使用模式匹配：

- Wildcard patterns
- Literal patterns
- Variable patterns
- Array patterns
- Constructor patterns
- Record patterns
- Named patterns

同时也支持 guard 和 模式 guard。

穷尽性检查器将为任何不穷尽的模式引入一个 `Partial` 约束。默认情况下，模式必须是详尽的，因为这个 `Partial` 约束不会被满足。然而，可以通过在函数中添加一个本地 `Partial` 约束来消除这个错误。

## 通配符模式（Wildcard pattern）

通配符 `_` 与任何输入相匹配，不会将任何东西带入作用域。

```purescript
f _ = 0
```

## 常量模式（Literal Pattern）

常量匹配提供匹配原语：

```purescript
f true = 0
f false = 1

g "Foo" = 0
g _ = 1

h 0 = 0
h _ = 1
```

## 变量模式（Variable Pattern）

变量模式匹配任何输入，并将该输入与它的名称绑定。

```purescript
double x = x * 2
```

## 数组模式（Array Pattern）

数组模式匹配一个数组的输入，并将其元素带入作用域。例如

```purescript
f [x] = x
f [x, y] = x * y
f _ = 0
```

这里，第一种模式只匹配长度为 1 的数组，并将数组的第一个元素带入作用域。

第二种模式匹配有两个元素的数组，并将第一个和第二个元素带入作用域。

## 构造器模式（Constructor patterns）

构造函数模式匹配一个数据构造函数及其参数。

```purescript
data Foo = Foo String | Bar Number Boolean

foo (Foo s) = true
foo (Bar _ b) = b
```

## Record 模式（Record Patterns）

Record模式匹配一个 Record 的输入，并将其属性带入作用域。

```purescript
f { foo: "Foo", bar: n } = n
f _ = 0
```

## 嵌套模式（Nested Patterns）

这种模式可以组合成建更大的模式。例如

```purescript
f { arr: [x, _], take: "firstOfTwo" } = x
f { arr: [_, x, _], take: "secondOfThree" } = x
f _ = 0
```

## 命名模式（Named Patterns）

命名模式在使用嵌套模式时，会将额外的名称带入作用于。任何模式都可以通过使用 `@` 符号来命名。

```purescript
f a@[_, _] = a
f _ = []
```

在第一种模式中，任何正好有两个元素的数组都将被匹配并绑定到变量 `a` 中。

## Guards

Guards 用于在模式内使用布尔值表达式施加额外的约束，并在模式后用管道引入。

```purescript
evens :: List Int -> Int
evens Nil = 0
evens (Cons x xs) | x `mod` 2 == 0 = 1 + evens xs
evens (Cons _ xs) = evens xs
```

当使用模式在顶层定义一个函数时，在所有模式之后都会出现 Guards。

```purescript
greater x y | x > y = true
greater _ _ = false
```

要想被认为是穷尽性的，Guards 必须清楚地包含一个始终为真的情况。尽管下面的代码很有意义，但编译器不能确定它是详尽的。

```purescript
compare :: Int -> Int -> Ordering
compare x y
    | x > y  = GT
    | x == y = EQ
    | x < y  = LT
```

因为它们显然包括一个最终案例，所以这些方法都可以：

```purescript
compare x y
    | x > y = GT
    | x < y = LT
    | otherwise = EQ

compare x y | x > y = GT
compare x y | x < y = LT
compare _ _ = EQ
```

(`otherwise` 是 Guards 中常用的`true`的同义词。)

## Pattern Guards

Pattern Guards 用一个左箭头表示模式匹配 Guards 的扩展。只有当箭头右边的计算与左边的模式匹配时，Pattern Guards 才会成功。

例如，我们可以将函数 `fn` 应用于一个参数 `x`，只有当 `fn` 对某个 `y` 返回 `Just y` 时才会成功，同时绑定 `y`。

```purescript
bar x | Just y <- fn x = ... -- x and y are both in scope here
```

当使用代数数据类型时，Pattern Guards 对于表达某些类型的控制流非常有用。

您也可以使用逗号在 Guards 中添加多个表达式。

```purescript
positiveLessThanFive :: Maybe Int -> Boolean
positiveLessThanFive mInt
  | Just x <- mInt
  , x > 0
  , x < 5 = true
  | otherwise = false
```
