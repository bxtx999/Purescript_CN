# `UnusedExplicitImport` 警告

## 举例

```purescript
module ShortFailingExample where

import Data.Maybe (maybe, Maybe(..))

test = Just -- maybe is not used
```

## 原因

A module is imported with an explicit imports list but not all of the identifiers are used.

## 修复

Remove the unnecessary identifiers as suggested.

## 备注

[This error is auto-fixable](../guides/Error-Suggestions.md).