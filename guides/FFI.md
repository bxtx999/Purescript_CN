# FFI

_Phil Freeman 2015-07-15_

在这篇文章中，我将展示如何使用 PureScript 的外部函数接口（或 FFI）与 Javascript 代码互操作。我们将看到如何从 PureScript 代码中调用 Javascript 代码，反之亦然。

#### 声明

应该说，选择通过 FFI 与 Javascript 一起工作，会在一定程度上“使类型检查器的保证失效”。一旦你走出 PureScript 类型系统的安全范围，什么都不能保证，所以建议你在编写自己的 FFI 绑定之前，先了解一些语言实现的基础知识。尽管如此，PureScript 类型和它们的 Javascript 表示之间的对应关系相当简单，所以应该不会太难理解。

#### 在 JavaScript 中调用 PureScript

从 JavaScript 代码中调用 PureScript 代码很简单，尤其是对于简单函数来讲。

让我们以下面这个简单的模块为例：

```purescript
module Test where

import Prelude

gcd :: Int -> Int -> Int
gcd n m | n == 0 = m
gcd n m | m == 0 = n
gcd n m | n > m = gcd (n - m) m
gcd n m = gcd (m - n) n
```

这个函数通过重复减法找到两个数字的最大公因子。这是一个很好的例子，在这种情况下，你可能想使用PureScript 来定义这个函数，但又要求从 Javascript 中调用它：在 PureScript 中定义这个函数很简单，因为它是由模式匹配和递归组成的，而且实现者可以从使用类型检查器中受益。

要了解如何从 Javascript 中调用这个函数，重要的是要意识到 PureScript 函数总是会变成单个参数的 Javascript 函数，所以我们需要逐一应用它的参数。

```javascript
var Test = require('Test');
Test.gcd(15)(20);
```

这里，我假设代码是用 `psc` 编译的，`psc` 将 PureScript 模块编译成 CommonJS 模块。因此，在使用 `require` 导入 `Test` 模块后，我能够在 `Test` 对象上引用 `gcd` 函数。

你也可能喜欢使用 `purs bundle` 为浏览器生成 JavaScript 代码。在这种情况下，你会在全局命名空间上访问 `Test` 模块，它默认为 `PS`。

```javascript
var Test = require('Test');
Test.gcd(15)(20);
```

#### 理解名字生成

PureScript 的目标是在代码生成过程中尽可能地保留名称。特别是，大多数既不是 PureScript 也不是 Javascript 关键字的标识符都可以被保留，至少对于顶层声明的名称是这样。

如果您决定使用 Javascript 关键字作为标识符，名称将用双元符号转义。例如：

```purescript
null = []
```

生成下面的 JavaScript 代码：

```javascript
var $$null = [];
```

此外，如果您想在您的标识符名称中使用特殊字符，它们将使用一个 `$` 进行转义。例如

```purescript
example' = 100
```

生成下面的 JavaScript 代码：

```javascript
var example$prime = 100;
```

#### 在 PureScript 中调用 JavaScript

通过使用 FFI，可以从 PureScript 中使用 Javascript 值和函数。问题在于如何为源自 Javascript 的值选择合适的类型。

关于类型的一般规则是，在使用 FFI 时，您可以根据自己的意愿实施少量或尽可能多的类型安全，但在处理 Javascript 值时，您应该注意避免常见的陷阱，例如 Javascript 函数返回空值或未定义值的可能性。Prelude 和核心库中定义的函数会尽可能地在类型安全方面下功夫。

#### 外部模块

在 PureScript 中， JavaScript 代码被包装成 `foreign module`。 `Foreign module` 只是一个与 PureScript 模块相关联的 CommonJS 模块。`Foreign module`需要遵守一定的约定：

- Foreign module 的名称必须与它对应的 PureScript 模块相同，扩展名改为 `.js`。这将 foreign module 与 PureScript 模块关联起来。
- 所有的输出必须是在顶层指定的 `exports.name = value;` 的形式。

下面是一个例子，我们从 foreign module 导出一个计算利息金额的函数。

```javascript
"use strict";

exports.calculateInterest = function(amount) {
  return amount * 0.1;
};
```

这个文件保存为 `src/Interest.js`，PureScript 对应的 `Interest` 模块保存在 `src/Interest.purs` 中。

```purescript
module Interest where

foreign import calculateInterest :: Number -> Number
```

在相应的 PureScript 模块中，我们只需通过使用 `foreign import` 声明为 foreign module 的导出值分配一个类型。然后，这些值就可以在 PureScript 模块使用 `import` 引入。

#### 多参数的函数

PureScript 函数默认柯理化处理，所以对于多参数的 JavaScript 函数需要特殊处理。

假设我们修改 `calculateInterest` 函数处理第二个参数：

```javascript
"use strict";

exports.calculateInterest = function(amount, months) {
  return amount * Math.exp(0.1, months);
};
```

现在，一个正确的 `foreign import` 声明应该能够在运行时使用 foreign 类型正确处理多个参数的函数。`purescript-functions` 软件包提供了从 0 到 10 的函数类型集合。

```purescript
module Interest where

import Data.Function.Uncurried (Fn2)

foreign import calculateInterest :: Fn2 Number Number Number
```

这里的 `Fn2` 类型构造函数用来包装两个参数的 Javascript 函数。我们可以在 PureScript 中写一个允许局部应用的柯理化包装函数。

```purescript
calculateInterestCurried :: Number -> Number -> Number
calculateInterestCurried = runFn2 calculateInterest
```

另一种方法是在原生模块中使用柯理化函数。按照函数类型构造函数（`->`）的运行时表示方式决定，使用多个嵌套函数，每个函数有一个参数。

```javascript
"use strict";

exports.calculateInterest = function(amount) {
  return function(months) {
    return amount * Math.exp(0.1, months);
  };
};
```

这次我们就可以直接分配柯理化的函数类型。

```purescript
foreign import calculateInterest :: Number -> Number -> Number
```

#### 处理约束类型

从 Javascript 中调用 PureScript 函数时应该注意的另一种特殊情况是：具有约束类型的值（即包含类型类约束的类型）包含额外的参数，这些参数用于向函数传递类型类字典。

例如，让我们写一个简单的带有约束类型的 PureScript 函数，然后看看生成的 Javascript。

```purescript
module Test where

import Prelude
import Data.Tuple (Tuple(..))

inOrder :: forall a. Ord a => a -> a -> Tuple a a
inOrder a1 a2 | a1 < a2 = Tuple a1 a2
inOrder a1 a2 = Tuple a2 a1
```

生成的 JavaScript 代码如下面的代码所示：

```javascript
var inOrder = function (__dict_Ord_32) {
  return function (_1) {
    return function (_2) {
      if (Prelude["<"](__dict_Ord_32)(_1)(_2)) {
        return Data_Tuple.Tuple(_1)(_2);
      };
      return Data_Tuple.Tuple(_2)(_1);
    };
  };
};
```

注意，`inOrder` 是一个由三个参数组成的(柯理化)函数，而不是两个参数。第一个参数是 `Ord` 约束的类型类字典。

我们可以从 Javascript 中调用这个函数，通过传递一个来自 Prelude 的显式类型类字典作为第一个参数。

```javascript
var test = Test.inOrder(Prelude.ordNumber())(20)(10);
```

#### 处理副作用

注意，上面定义的 `calculateInterest` 函数是*纯粹*的：它们没有副作用，并且每次调用都会对相同的输入产生相同的结果。

PureScript 函数类型 `a->b` 不允许有副作用，所以将函数类型分配给有副作用的 Javascript 计算是不正确的。在这种情况下，正确的方法是使用在  `purescript-effect` 包中定义的 `Effect` 类型构造函数为计算分配一个类型。

`Effect` 类型构造函数及其用法在 [Pursuit](https://pursuit.purescript.org/packages/purescript-effect) 上有相关的文档。

#### 利用 `Data.Foreig` 对 Foreign 数据消毒

从 Javascript 函数返回的数据一般不能相信是 defined 和 non-null 的。Prelude 常用库中的 PureScript 函数一般都假定值既不会是未定义的，也不会是空的，所以在使用 FFI 处理 Javascript 函数返回的值时，对数据进行消毒很重要。

`Data.Foreign` 模块（在 `purescript-foreign` 包中）定义了一个 `Foreign` 数据类型，以及几个用于将 `Foreign` 值转化为常规 PureScript 值的辅助函数，并支持使用 `Maybe` 类型构造函数处理 `null` 和 `undefined`。

#### 定义 Foreign 数据类型

在包装 Javascript APIs 时，创建特定种类的新类型供 FFI 使用往往很有用。

例如，假设我们有一个 Javascript 库 `frob`，它定义了 `Frob` 数据结构和相关函数。为了给这些函数赋予有意义的类型，可能需要在种类 `Type` 处定义一个 `Frob`类型。我们可以这样做：

```purescript
foreign import data Frob :: Type
```

`Frob` 类型现在可以在其他类型中使用，也可以在 `foreign import` 中声明：

```purescript
foreign import makeFrob :: String -> Frob
```

定义外部数据类型的开发人员应注意记录数据在运行时的预期表示形式。

#### 结语

这篇文章希望告诉你只要了解了一些小的实现细节，与 Javascript 的双向互操作都很简单。你现在应该能够将你的 Javascript 库包在 PureScript 中使用，反之亦然。

[PureScript 书](https://book.purescript.org/chapter10.html)中包含了更多关于 FFI 的信息，以及大量的例子和练习，供感兴趣的读者参考。
