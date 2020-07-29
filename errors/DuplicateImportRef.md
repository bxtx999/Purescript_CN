# `DuplicateImportRef` 警告

## 举例

```purescript
module ShortFailingExample where

import Prelude (unit, unit)

test = unit
```

## 原因

An explicit import list contains multiple references to the same thing, i.e. has duplicates.

## 修复

Remove the extraneous items.

## 备注
