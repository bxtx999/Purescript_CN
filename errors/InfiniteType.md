# `InfiniteType` 错误

## 举例

```purescript
module ErrorExample where

applySelf x = x x
```

## 原因

This fails because x must have a function type, but the domain of the function must be the same type, so we end up with an infinite type `(((... -> r) -> r) -> r) -> r`.
