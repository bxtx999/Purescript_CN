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

### 数字类型

数字类型可以是整型数（`Int`类型）或者是浮点数（`Number`类型）。浮点数类型使用小数点标识。采用16进制的整型数前面应该使用 `0x`。

```purescript
16 :: Int
0xF0 :: Int
16.0 :: Number
```

### 字符串类型

字符串用双引号括起来，可以扩展到多行。向下面的代码一样使用换行符 `\` 将字符串包起来：

```purescript
"Hello World"

"Hello \
\World"
```

#### 三引号字符串

如果输出中需要换行，可以用 `\n` 换行符。另外，你也可以使用三个双引号来防止转义符号的特殊解析。这也允许在字符串中使用双引号而无需转义。

```purescript
jsIsHello :: String
jsIsHello = """
function isHello(greeting) {
  return greeting === "Hello";
}
"""
```

在编写正则表达式字符串时，这种声明字符串的方法特别有用。

```purescript
regex ".+@.+\\..+" noFlags
regex """.+@.+\..+""" noFlags
```

上面的正则表达式代码是一个非常简单的电子邮件地址验证器。 两行代码等效的，但第二个使用三重双引号，则更容易编写和维护。 当编写复杂的正则表达式时，也更要这么做。

### 布尔类型

布尔值为 `true` 和 `false`。

### 函数

函数值（有时也被称为 lambda）通过使用反斜杠和参数名列表来引入。

```purescript
\a b -> a + b
```

上面的代码与下面的 JS 代码等效。

```javascript
function (a) {
  return function (b) {
    return a + b;
  }
}
```

### 数组

与 JavaScript 一样，数组变量用 `[]` 表示：

```purescript
[]
[1, 2, 3]
```

### Record

与 JS 中类似，Record 变量通过 `{}` 标识。

```purescript
{}
{ foo: "Foo", bar: 1 }
```

带通配符的 Record 变量可以用来创建一个产生 Record 的函数。

```purescript
{ foo: _, bar: _ }
```

等同于：

```purescript
\foo bar -> { foo: foo, bar: bar }
```

## 其他形式的 Record

### 属性访问器

就像在JavaScript中一样，访问 Record 的一个属性，使用点加属性名。

```purescript
rec.propertyName
```

还有一些部分应用的访问器，在属性名后面加下划线。

```purescript
_.propertyName
```

这等价于：

```purescript
\rec -> rec.propertyName
```

可以在各个层级中使用。

```purescript
_.nested.property.name
```

### Record 更新

可以使用以下语法更新 Record 的属性。

```purescript
rec { key1 = value1, ..., keyN = valueN, nestedKey { subKey = value, ... } }
```

部分或全部键值可以同时更新，Record 中的 Record 也可以更新。

例如，下面的函数对其参数 `foo` 属性进行自增操作。

```purescript
\rec -> rec { foo = rec.foo + 1 }
```

[嵌套 Record 这样更新](https://liamgoodacre.github.io/purescript/records/2017/01/29/nested-record-updates.html):

```purescript
r = { val: -1
    , level1: { val: -1
              , level2: { val: -1 }
              }
    }
r' = r { level1 { val = 1 } }
```

通配符还可以在更新中使用来生成部分应用的更新

```purescript
rec { foo = _ }
```

等价于：

```purescript
\foo -> rec { foo = foo }
```

下划线也可以出现在更新器的对象位置。

```purescript
_ { foo = 1 }
```

等价于：

```purescript
\rec -> rec { foo = 1 }
```

## 二元操作符

PureScript 中的运算符只是常规的二元函数。特别是，PureScript 语言中没有内置运算符；因此，对 Prelude 等库中定义的运算符的概述不在本参考资料的范围之内。

操作符可以通过为现有函数提供一个操作符别名来定义（该别名必须是二元的，即其类型必须是 `a -> b -> c` 的形式）。例如：

```purescript
data List a = Nil | Cons a (List a)

append :: forall a. List a -> List a -> List a
append xs Nil = xs
append Nil ys = ys
append (Cons x xs) ys = Cons x (append xs ys)

infixr 5 append as <>
```

这个函数可以这样使用：

```purescript
oneToThree = Cons 1 (Cons 2 (Cons 3 Nil))
fourToSix = Cons 4 (Cons 5 (Cons 6 Nil))

oneToSix = oneToThree <> fourToSix
```

操作符别名声明由四部分组成：

- 结合性：`infixl`、`infixr` 或 `infix`；
- 优先级：由 0 到 9 之间的整数表示，这里使用 5；
- 函数别名：这里是 `append`；
- 操作符：这里是 `<>`。

该声明决定了涉及该操作符的表达式如何用括号表示。

### 结合律（Associativity）

`infixl` 的意思是重复的应用是从左边开始用括号括起来的。例如，Prelude 中的 `#` 是左关联的，这意味类似下面这样的表达式：

```purescript
products # filter isInStock # groupBy productCategory # length
```

括号内是：

```purescript
((products # filter isInStock) # groupBy productCategory) # length
```

同样，`infixr` 的意思是 "右关联"，重复的应用从右开始用括号表示。例如，来自 Prelude 的 `$` 是右联，所以表达式为：

```purescript
length $ groupBy productCategory $ filter isInStock $ products
```

括号内是：

```purescript
length $ (groupBy productCategory $ (filter isInStock $ products))
```

`infix`的意思是 "非关联性",不允许重复使用非关联性操作符。例如，来自 Prelude 的 `==` 是非关联性的，这意味着：

```purescript
true == true == true
```

导致一个[NonAssociativeError](https://github.com/purescript/documentation/blob/master/errors/NonAssociativeError.md)

```bash
Cannot parse an expression that uses multiple instances of the non-associative operator Data.Eq.(==).
Use parentheses to resolve this ambiguity.
```

通过 `infix` 进行非关联性解析最适合运算符 `f`，其中 ``x `f` (y `f` z)`` 不一定与 ``(x `f` y)`` 相同，即不满足关联性代数性质的运算符。

### 优先级（Precedence）

优先级决定了括起来的运算符的顺序。优先级较高的操作符将在前面用括号括起来。例如，在 Prelude 中取 `*` 和 `+`。`*` 优先于 `7`，而 `+` 优先于 `6`。因此，如果我们写：

```purescript
2 * 3 + 4
```

然后将其括起来如下：

```purescript
(2 * 3) + 4
```

不同关联性的操作符只要不具有相同的优先级，就可以同时出现。这个限制之所以存在，是因为编译器无法合理地选择如何给这些表达式加括号。例如，Prelude 中的运算符 `==` 和 `<$>` 分别具有固定性 `infix4` 和 `infixl 4`，这意味着给定表达式为

```purescript
f <$> x == f <$> y
```

编译器不知道是否将其括起来

```purescript
(f <$> x) == (f <$> y
```

或

```purescript
f <$> (x == f) <$> y
```

因此，我们得到了一个 [MixedAssociativityError](https://github.com/purescript/documentation/blob/master/errors/MixedAssociativityError)

```bash
Cannot parse an expression that uses operators of the same precedence but mixed associativity:

  Data.Functor.(<$>) is infixl
  Data.Eq.(==) is infix

Use parentheses to resolve this ambiguity.
```

### 作为值的运算符( Operators as Values )

运算符可以用括号括起来作为普通值：

```purescript
and = (&&)
```

### 操作符部分

通过用括号将运算符括起来并使用 `_` 作为操作数之一，可以部分应用运算符：

```purescript
half = (_ / 2)
double = (2 * _)
```

### 函数作为运算符（Functions as Operators）

当函数被反引号包围时，它们可以用作中缀运算符：

```purescript
foo x y = x * y + y
test = 10 `foo` 20
```

运算符部分也适用于以这种方式使用:

```purescript
fooBy2 = (_ `foo` 2)
```

## Case 表达式

关键字 `case` 和 `of` 用于解构值，以创建基于值的构造函数的逻辑。你可以通过在 header 和 cases 中用 `,` 来限定多个值的匹配。

```purescript
f :: Maybe Boolean -> Either Boolean Boolean -> String
f a b = case a, b of
  Just true, Right true -> "Both true"
  Just true, Left _ -> "Just is true"
  Nothing, Right true -> "Right is true"
  _, _ -> "Both are false"
f (Just true) (Right true)
```

像顶级声明一样，`case` 表达式支持防护。

```purescript
f :: Either Int Unit -> String
f x = case x of
  Left x | x == 0 -> "Left zero"
         | x < 0 -> "Left negative"
         | otherwise -> "Left positive"
  Right _ -> "Right"
```

可以通过使用一个下划线代替表达式来避免绑定；在这种情况下，下划线代表一个*匿名参数*。

```purescript
case _ of
  0 -> "None"
  1 -> "One"
  _ -> "Some"
```

等价于

```purescript
\x -> case x of
  0 -> "None"
  1 -> "One"
  _ -> "Some"
```

## If-Then-Else 表达式

关键字 `if`、`then` 和 `else` 可用于创建类似于 JavaScript 三元表达式的条件表达式。注意，表达式始终需要 `else` 块。

```purescript
conditional = if 2 > 1 then "ok" else "oops"
```

## Let 和 Where 绑定

`let` 关键字引入了局部声明的集合，这些声明可以相互递归，并且可以包括类型声明：

```purescript
factorial :: Int -> Int
factorial =
  let
    go :: Int -> Int -> Int
    go acc 1 = acc
    go acc n = go (acc * n) (n - 1)
  in
    go 1
```

`where` 关键字还可用于在值声明的末尾引入局部声明：

```purescript
factorial :: Int -> Int
factorial = go 1
  where
  go :: Int -> Int -> Int
  go acc 1 = acc
  go acc n = go (acc * n) (n - 1)
```

## 绑定快的缩进

绑定体的缩进是很重要的，如果定义了多个绑定，比如在 let-in 块中，每个绑定必须有相同的缩进程度。如果定义多个绑定，如在一个 let-in 块中，每个绑定都必须有相同程度的缩进。那么，绑定的定义主体必须进一步缩进。

举个例子：

```purescript
f =
  -- The `let-in` block starts at an indentation of 2 spaces, so
  --   the bindings in it must start at an indentation greater than 2.
  let
    -- Because `x` is indented 4 spaces, `y` must also be indented 4 spaces.
    -- Its body, then, must have indentation greater than 4 spaces.
    x :: Int -> Int
    x a =
      -- This body is indented 2 spaces.
      a
    y :: Int -> Int
    y c =
        -- This body is indented 4 spaces.
        c
  in do
    -- Because `m` is indented 4 spaces from the start of the `let`,
    --   `n` must also be indented 4 spaces.
    -- Its body, then, must be greater than 4 spaces.
    let m =
          -- This body is indented 2 spaces.
          x (y 1)
        n =
            -- This body is indented 4 spaces.
            x 1
    log "test"
```

## Do

关键字 `do` 为单项表达式引入了简单的语法糖。

下面是一个例子，使用单项式来表示 `Maybe` 类型。

```purescript
maybeSum :: Maybe Number -> Maybe Number -> Maybe Number
maybeSum a b = do
  n <- a
  m <- b
  let result = n + m
  pure result
```

`maybeSum` 接收两个类型为 `Maybe Number` 的值，如果两个值都不是 `Nothing`，则返回它们的和。

语句可以有以下形式：

- `a <- x` 是 `bind x \a -> ...` 的语法糖。
- `x` 是 `bind x \_ -> ...` 的语法糖。如果这是最后一个语句，则只需 `x`。
- 一个 let 绑定 `let a = x`。注意，缺少 `in` 关键字。

例子 `maybeSum` 去糖化为::

```purescript
maybeSum a b =
  bind a \n ->
    bind b \m ->
      let result = n + m
      in pure result
```

在实际使用中，您通常会使用 [Prelude 中的 `bind`](https://pursuit.purescript.org/packages/purescript-prelude/docs/Control.Bind#v:bind)，但是去糖化将在作用域内使用 `bind`。当使用 Prelude 的 `bind` 时，必须有一个 `Monad` 类型类的实例作为返回类型。
