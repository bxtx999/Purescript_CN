# `UnusedDctorImport` 警告

## 举例

```purescript
module ShortFailingExample where

import Data.Maybe (Maybe(..))

test :: forall a. Maybe a -> Int
test _ = 42
```

## 原因

A module is imported with an explicit imports list including a type with data construtors (`Maybe(..)` in this example)
but only the type constructor and not its data constructors are used.

## 修复

Import only the type without data constructors. In the above example:

```purescript
import Data.Maybe (Maybe)
```

## 备注

[This error is auto-fixable](../guides/Error-Suggestions.md).
