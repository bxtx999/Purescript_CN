# 语法

## 空格规则

语法是对空格敏感的。一般的经验法则是，跨越多行的声明应该在其后的行中缩进到它们第一次定义的那一列之后。

也就是说，以下内容是有效的。

```purescript
foo = bar +
  baz
```

下面这样是不允许的。

```purescript
foo = bar +
baz
```

## 注释

单行注释使用 `--`：

```purescript
-- This is a comment
```

多行注释使用 `{- -}`

```purescript
{-
  Comment
  continued comment
-}
```

文档以管道符号 `|` 开始，并可以通过 `psc-docs` 和 Pursuit 等工具的输出。例如：

```purescript
-- | `bool` performs case analysis for the `Boolean` data type, like an `if` statement.
bool :: forall a. Boolean -> a -> a -> a
bool true x _ = x
bool false _ x = x
```

与 Haskell 不同，文档的每一行都应该以管道符号 `|` 开始。例如：

```purescript
-- | Sort an array based on its `Ord` instance.
-- |
-- | This implementation runs in `O(n^2)` time, where `n` is the length of the
-- | input array.
-- TODO: try to optimise this?
sort :: forall a. (Ord a) => Array a -> Array a
sort xs = [...]
```

## 顶层声明

模块顶层的值是通过提供名字和等号来定义，然后是要关联的值。

```purescript
one = 1
```

这里可以使用的模式种类请参见模式匹配一节。

使用模式匹配的函数可以被多次定义，以处理不同的模式匹配。

```purescript
isEmpty [] = true
isEmpty _ = false
```

但这并不意味着函数可以任意重载不同数量或类型的参数。

在这些定义中也可以使用护卫。

```purescript
isEmptyAlt xs | length xs == 0 = true
isEmptyAlt _ = false
```

通常使用类型签名定义顶级声明：

```purescript
multiply :: Number -> Number -> Number
multiply x y = x * y
```

一般来说，顶层声明不需要类型签名，但最好的做法是这样做。详见类型一节。

## 函数与参数名

函数和值名称必须以小写字母（[unicode类别](https://en.wikipedia.org/wiki/Unicode_character_property#General_Category) `Ll`）或下划线 `_` 开头。以下字符可以是任意数量：

- Letter: Unicode category `L`
- Mark: Unicode category `M`
- Number: Unicode category `N`
- Underscore: `_`
- Apostrophe: `'`

## 函数应用

函数应用只是通过函数与其参数的并列关系来表示。

```purescript
add 10 20
```

PureScript 函数是柯里化的，所以局部应用没有特殊的语法。

```purescript
add10 = add 10
```

实际上，`add 10 20` 解析为 `(add 10) 20`。

## 常量

### Number

## String
