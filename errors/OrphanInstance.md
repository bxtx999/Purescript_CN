# `OrphanInstance` 错误

## 举例

```purescript
module A where

class SomeClass a
```

```purescript
module B where

data SomeData
```

```purescript
module C where

import A
import B

instance someInstance :: SomeClass SomeData
```

## 原因

This error occurs when an instance is declared outside of the module that declares the class it is for, and also none of the instance's types are declared in the same module.

Above, `someInstance` is an orphan here as it is defined in a module separate from both `SomeData` and `SomeClass`.

## 修复

- If possible, move the instance into an allowed module.
- Consider adding a `newtype`, and defining the instance in the same module as the `newtype`.

## 备注

### Orphan Instances with Functional Dependencies

When using type classes involving functional dependencies, the rules for orphan instances are a little more complicated. They are explained in [this blog post](https://liamgoodacre.github.io/purescript/type/class/instance/orphan/functional/dependencies/2017/01/22/purescript-orphan-instance-detection.html).
