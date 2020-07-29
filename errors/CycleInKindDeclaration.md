# `CycleInKindDeclaration` 错误

## 原因

A top-level kind signature is declared in a way that would incur a dependency on itself.

## 修复

- A kind signature cannot depend on itself.
- Check the kind signature for any types mentioned in the kind signature.
