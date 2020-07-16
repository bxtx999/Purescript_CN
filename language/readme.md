# Purescript 语言指南

这是 Purescript 的入门实例，这里以"hello world"为例。

```purescript
module Main where

impport Effect.Console

main = log "Hello, World!"
```

### 另一个例子

下面的例子定义数据类型 `Person` 和 `Person` 生成字符串表示的函数。

```purescript
data Person = Person { name :: String, age :: Int }

showPerson :: Person -> String
showPerson(Person o) = show o.name <> ", aged " <> show o.age

examplePerson :: Person
examplePerson = Person { name: "Bonnie", age: 26 }
```

逐行解释：

- `Person` 是具有构造函数的数据类型。
- `Person` 构造函数具有两个属性，`String` 类型的 `name` 和 `Int` 类型的 `age`。
- `showPerson` 函数获取参数 `Person` 并返回一个 `String`。
- `showPerson` 对参数进行个案分析，首先匹配构造函数 `Person`，然后使用字符串串联和对象访问器返回其结果。
- `examplePerson`是一个 `Person` 对象，由 `Person` 构造函数构成，并为名称赋值 `String` "Bonnie"，为年龄赋值 `Int` 26。

### 指南

1. [Types](Types.md)
1. Syntax
1. Type Classes
1. Pattern Matching
1. Modules
1. FFI
1. Records
1. Differences from Haskell