# `ImportHidingModule` 错误

## 举例

```purescript
module ShortFailingExample where

import B hiding (module A)
```

## 原因

An `import` statement has a `hiding` list containing a `module` reference. Hiding imports cannot be used to hide modules.

## 修复

Remove the `hiding` item.

## 备注
