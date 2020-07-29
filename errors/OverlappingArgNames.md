# `OverlappingArgNames` 错误

## 举例

```purescript
> f x x = x
Error found:
in module $PSCI
at line 1, column 5 - line 1, column 13

  Overlapping names in function/binder in declaration f
```

## 原因

This error occurs when you attempt to bind the same variable to different function arguments.

## 修复

- Rename one of the duplicate function arguments.

## 备注
