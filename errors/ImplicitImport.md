# `ImplicitImport` 警告

## 举例

```purescript
module ShortFailingExample where


import Prelude
import Data.Maybe

z = Just (-1)
```

## 原因

A module is imported implicitly (without an explicit import list), and there is another implicit import present (which may or may not be `hiding`).

## 修复

Make all but at most one import explicit. In the above example we could make either `Data.Maybe` or `Prelude` explicit, e.g.

```
import Prelude (negate)
```

## 备注

[This error is auto-fixable](../guides/Error-Suggestions.md).