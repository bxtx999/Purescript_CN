# `UnknownImport` 错误

## 举例

```
> import Prelude (doesNotExist)
Error found:
in module $PSCI
at line 1, column 17 - line 1, column 29

  Cannot import value doesNotExist from module Prelude
  It either does not exist or the module does not export it.
```

## 原因

This error occurs when trying to import a name which does not exist.

## 修复

- Check spelling of any imported names.
- Check dependencies to make sure you are using the correct versions. You can use [Pursuit](https://pursuit.purescript.org/) to check which package versions define the names you are trying to import.

## 备注
