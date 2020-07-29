# `OrphanKindDeclaration` 错误

## 举例

```purescript
data Proxy :: forall k. k -> Type
```

## 原因

A top-level kind signature is provided but not immediately followed by it's declaration.

## 修复

- Check that you've provided the appropriate `data`, `newtype`, `type`, or `class` declaration.
- Check that the keywords match.
