# `UnsupportedTypeInKind` 错误

## 举例

```purescript
foreign import data Bad :: SomeConstraint => Type
```

## 原因

Not all types are valid in a kind signature. Particularly constraint arrows
(`=>`) are disallowed since they only make sense for term-level constraint
dependencies.

## 修复

- Only use types that are valid in kinds.
