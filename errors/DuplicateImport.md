# `DuplicateImport` 警告

## 举例

```purescript
module ShortFailingExample where

import Data.Maybe
import Data.Maybe

test = Nothing
```

## 原因

The same module has been imported multiple times (with the same qualification if any).

## 修复

Remove one of the extra import statements.

## 备注

[This error is auto-fixable](../guides/Error-Suggestions.md).
