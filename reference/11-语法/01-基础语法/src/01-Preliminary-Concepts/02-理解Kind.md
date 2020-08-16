# 理解 Kind

看下面这个代码：

```purescript
function :: Int -> String
function x = "an integer value!"
```

...翻译成“在你给我一个 Int 值之前，我不能给你一个具体的值(String)”

```purescript
data Box a = Box a
```

0...翻译成：“在你告诉我 a 是什么之前，我不能给你一个具体的类型（例如 Box Int，一个存储 Int 值（而不是 String 值或其他值）的盒子）。”

## Kind 是什么

> Kinds = “在我拥有一个‘具体’类型之前，我还需要定义多少个类型？”^^。

^^这是一个“工作定义”。在考虑类型级编程时，还有更多的内容，但现在，这就足够了。

| # 仍然需要定义的类型 | 特殊名称 | 它们的 "Kind" signature (Purescript)^^ | 它们的 "Kind" signature (Haskell)^^
| - | - | -: | -: |
| 0 | Concrete Type             | `                Type` | `          *`
| 1 | Higher-Kinded Type (by 1) | `        Type -> Type` | `     * -> *`
| 2 | Higher-Kinded Type (by 2) | `Type -> Type -> Type` | `* -> * -> *`
| n | Higher-Kinded Type (by n) | `... Type ... -> Type` | `... * ... -> *`

^^这些列是右对齐的，以显示最后的 `Type`/`*` 是 "concrete" 类型。另外，`... Type ... -> Type`(及其 Haskell 的等价物)语法不是实语法，只是在 n-kinded 类型中传达了递归思想。其他三种  (0 - 2) 都是实实在在的语法。

## Concrete Types

通常可以用字面值编写：

```purescript
(1 :: Int)
("a literal string" :: String)

([1, 2, 3] :: Array Int)
-- Although Array is box-like, we know the values it stores
-- Thus, is still has a concrete type
```

## Higher-Kinded 类型

Higher-kinded 类型是指还需要一种或多种类型来定义。

```purescript
-- Kind: Type -> Type
-- Reason: the `a` type needs to be defined
data Box a = Box a

-- As we can see, there can be many different concrete 'Box' types
-- depending on what 'a' is:
boxedInt :: Box Int
boxedInt = Box 4

boxedString :: Box String
boxedString = Box "string"

boxedBoxedInt :: Box (Box Int)
boxedBoxedInt = Box boxedInt
```

我们可以通过添加更多需要指定的类型，使类型的 kind 更高。比如说：

```purescript
-- A box that holds two values of same or different types!
data BoxOfTwo a b = BoxOfTwo a b

-- The below syntax is not valid because it is missing `forall a b.`,
--   but it gets the idea across. "Forall" syntax will be covered later.
higherKindedBy2 :: a -> b -> BoxOfTwo a b
higherKindedBy2 a b = BoxOfTwo a b

-- We can lower the kind by specifying one of the data types:
higherKindedBy1L :: b -> BoxOfTwo Int b
higherKindedBy1L b = BoxOfTwo 3 b

higherKindedBy1R :: a -> BoxOfTwo a String
higherKindedBy1R a = BoxOfTwo a "a string value"

concreteType :: BoxOfTwo Int String
concreteType = BoxOfTwo 3 "a string value"
```

通用类型也可以对类型的值进行拆分：

```purescript
-- It's either an A or it's a B, but not both!
data Either a b
  = Left a
  | Right b

higherKindedBy2L :: a -> b -> Either a b
higherKindedBy2L a b = Left a

higherKindedBy2R :: a -> b -> Either a b
higherKindedBy2R a b = Right b

higherKindedBy1L_ignoreB :: b -> Either Int b
higherKindedBy1L_ignoreB b = Left 3

higherKindedBy1L_useB :: b -> Either Int b
higherKindedBy1L_useB b = Right b

higherKindedBy1L_ignoreBoth :: a -> b -> Either Int b
higherKindedBy1L_ignoreBoth a b = Left 3
```

`Either`（其中 `a` 和 `b` 还没有指定）有 `Type -> Type -> Type` kind，因为在 `a` 和 `b` 类型都被定义之前，它不能成为一个具体的类型，即使只构造其通用类型已知的一个值。

## 类型推导表

|  | 推导的 kind |
|-|-:|
|`Unit`|`Type`|
|`Array Boolean`|`Type`|
|`Array`|`Type -> Type`|
|`Either Int String` | `Type`|
|`Either Int` | `Type -> Type`|
|`Either` | `Type -> Type -> Type`|
|...|...|
