# `UnusedImport` 警告

## 举例

```purescript
module ShortFailingExample where

import Some.Module

-- Nothing from Some.Module is ever used
```

## 原因

There is a statement `import Some.Module` which is not required, as nothing from `Some.Module` is used (with the same qualifier if any).

## 修复

Remove the import statement.

## 备注

Often occurs during development when the code using the import has not yet been written, in which case can be ignored. 

[This error is auto-fixable](../guides/Error-Suggestions.md).
