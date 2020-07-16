# Types

类型系统定义如下：

- 原生类型: `Int`, `Number`, `String`, `Char`, `Boolean`
- 数组(Arrays)
- 记录(Records)
- Tagged Unions
- Newtypes
- Functions
- Polymorphic Types
- Constrained Types
- Type Synonyms
- Rows

### 基础类型

基础类型 `String`、`Numer` 和 `Boolean` 与 Javascript 一致。

```purescript

$ spago repl
> :type 1.0
Number

> :type "test"
String

> :type true
Boolean
```

### 整型数

`Int` 类型表示整数值,在运行时也表示为 JavaScript 中的`Number`。但是，对`Int`值上的操作（`+`）的定义不同，以确保始终获得`Int`类型的值。

```purescript

$ spago repl
> :type 1
Int
```
### 字符
`Char` 类型

```purescipt

$ spago repl
> :type 'a'
Char
```

### 数组

`Array`类型对应 Javascript 中的 `Array` 类型，但数组中所有元素都应是相同类型的。

```purescript

$ spago repl
> :type [1, 2, 3]
Array Int

> :type [1, false]
Could not match type Int with type Boolean.
```

### 记录

记录对应 Javascript 中的 object。
   
```purescript

> author = { name: "Phil", interests: ["Functional Programming","JavaScript"] }

> :type author
{ interests :: Array String
， name :: String
}
```

