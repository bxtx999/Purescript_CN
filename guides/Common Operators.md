# 通用操作符

JS的`<X operator>`与 PureScript 哪些等价？

虽然 PureScript 语言没有定义内置的运算符，但在 `Prelude` 和其他核心库中定义了大量的运算符。本指南将为您概述这些运算符以及 JavaScript 中相应的运算符。

## JavaScript 的一元操作符

所有的PureScript运算符都是二元的，但有一个例外：有语法糖用于单义否定，在PureScript代码中，`-x`会被去掉，变成`negate x`，它使用范围内的任何一个`negate`。

这意味着，PureScript中的单义JavaScript运算符（除了`-`）的类似物不是运算符，而是普通函数，必须使用标准的前缀式函数应用，因此，例如，JS中的`！x`在PureScript中必须写成`not x`。


| JS 操作符 | PureScript 函数 | 定义            | 含义          |
|-------------|---------------------|-----------------------|------------------|
| `-`         | `negate`            | `Data.Ring`           | Numeric negation |
| `!`         | `not`               | `Data.HeytingAlgebra` | Boolean negation |
| `~`         | `complement`        | `Data.Int.Bits`       | Bitwise negation |


## JavaScript的二元操作符

| JS 操作符 | PureScript 函数 | 定义            | 含义                                  |
|-------------|---------------------|-----------------------|------------------------------------------|
| `+`         | `+`                 | `Data.Semiring`       | Numeric addition                         |
| `+`         | `<>`                | `Data.Semigroup`      | String concatenation                     |
| `-`         | `-`                 | `Data.Ring`           | Numeric subtraction                      |
| `*`         | `*`                 | `Data.Semiring`       | Numeric multiplication                   |
| `/`         | `/`                 | `Data.EuclideanRing`  | Numeric division                         |
| `%`         | `%` (see note)      | `Math`                | Remainder, the same as JS' `%` operator. |
| `%`         | `mod` (see note)    | `Data.EuclideanRing`  | Also remainder (see note)                |
| `==`        | `==`                | `Data.Eq`             | Equality check                           |
| `!=`        | `/=`                | `Data.Eq`             | Inequality check                         |
| `<`         | `<`                 | `Data.Ord`            | Less than                                |
| `<=`        | `<=`                | `Data.Ord`            | Less than or equal                       |
| `>`         | `>`                 | `Data.Ord`            | Greater than                             |
| `>=`        | `>=`                | `Data.Ord`            | Greater than or equal                    |
| `&&`        | `&&`                | `Data.HeytingAlgebra` | Boolean AND                              |
| `\|\|`      | `\|\|`              | `Data.HeytingAlgebra` | Boolean OR                               |
| `&`         | `.&.`               | `Data.Int.Bits`       | Bitwise AND                              |
| `\|`        | `.\|.`              | `Data.Int.Bits`       | Bitwise OR                               |
| `^`         | `.^.`               | `Data.Int.Bits`       | Bitwise XOR                              |
| `<<`        | `shl`               | `Data.Int.Bits`       | Shift Left                               |
| `>>`        | `shr`               | `Data.Int.Bits`       | Shift Right                              |
| `>>>`       | `zshr`              | `Data.Int.Bits`       | Zero-fill Shift Right                    |

这些函数和运算符大多由 "Prelude "重新导出，所以要在你的代码中使用它们，你通常应该写 `import Prelude`。

此外，这些函数和运算符中的许多都是在类型类中定义的，可以与许多不同的类型一起工作。例如，`+` 和 `*` 不仅适用于 `Int` 和 `Number`，而且适用于 `Prelude` 中定义的 `Semiring` 类型类的任何实例。

### 关于余数/取模的说明(`%`)

通常，当我们要求一个数除以另一个数后的余数时，我们谈论的是整数。显然，10除以3后的余数是1，事实在JS中，`10 % 3 == 1`。同样，在PureScript中，``10 `mod` 3 == 1``。

不过，如果我们想把'余数'操作扩展到实数(非积分)，那就有点难说了。实数之间可以互除，没有任何'剩余'，也就是说，在某种意义上，一个数除以另一个数的剩余总是0。

由于JS只有一个数字类型，它必须想出一个合理的方法来处理它的一个或两个参数都是非积分的情况。为了解决这个问题，它使用了一种行为，即它试图找到第二个参数比第一个参数小的最大*整数*倍，然后返回它们之间的差值。例如，在JS中，`10.5 % 3 == 1.5`。

如果这是你在PureScript中想要的行为，你应该使用`purescript-math`中`Math`模块中的`%`。它的类型是 "Number -> Number -> Number"，它只是简单地委托给JS中的"%"运算符。

然而，PureScript的`Prelude`旨在为像这样的常用运算符提供更强大的理论基础，这也是为什么`Prelude`导出了一个稍微不同的函数，`mod :: forall a. EuclideanRing a => a -> a -> a`。对于整数，`mod`的工作方式是你所期望的。``10 `mod` 3 == 1``，就像之前一样。然而，对于Numbers，`mod`总是返回0。这可能会让人感到惊讶；然而，它以这种方式工作的原因是，它是基于一个数学结构，称为*欧氏环*，其定义要求这种行为。更多信息，请参见[EuclideanRing文档](https://pursuit.purescript.org/packages/purescript-prelude/4.1.0/docs/Data.EuclideanRing#t:EuclideanRing)。
