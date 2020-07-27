# Records

与 JavaScript 一样，Record 常量使用大括号（`{}`）标注。

```purescript
author :: { name :: String, interests :: Array String }
author =
    { name: "Phil"
    , interests: ["Functional Programming", "JavaScript"]
    }
```

使用`点字段`名来访问 record。

```purescript
> author.name
"Phil"

> author.interests
["Functional Programming","JavaScript"]
```

## Kind

`{ ... }` 只是 Record 类型构造函数的语法糖，所以 `{ language :: String }` 和 `Record ( language :: String )` 是一样的。

Record 类型构造函数的参数是由一排类型组成的。在 Kind 符号中，Record 有 Kind `# Type -> Type`。也就是说，它把 row 类型变成一个类型。

`( language :: String )` 表示 row 类型(kind `# Type`)，所以它可以传递给 Record 构造一个类型，即 `Record ( language :: String )`。

## 扩展 Record

扩展 Record 可以这样定义：

```purescript
type Lang l = { language :: String | l }
```

可以扩展为：

```purescript
type Language = Lang ( country :: String )
```

`Language` 类型等同于 `{ language :: String, country :: String }`。请注意，扩展必须使用括号，因为 `l` 必须是 row 类型而不是 record 类型。

## 通配符

带通配符的 Record 常量可以用来创建一个 record 生成函数。

```purescript
{ foo: _, bar: _ }
```

等同于：

```purescript
\foo bar -> { foo: foo, bar: bar }
```

## Record 更新

PureScript 还提供了类似 Haskell 的 record 更新语法。

```purescrpt
setX :: Number -> Point -> Point
setX val point = point { x = val }
```

这可用于更新嵌套的 record：

```purescript
setPersonPostcode :: PostCode -> Person -> Person
setPersonPostcode pc p = p { address { postCode = pc } }
```

也可以通过使用 `_` 代替要更新的 record 来定义 record 更新函数，比如。

```purescript
_ { fieldName = newValue }
```

## 字段名

属于非法值标识符的符号（如标题首字母标识符或含有空格的符号）可以用双引号将其括起来来标识一个字段。

```purescript
author' :: { "Name" :: String, "Personal Interests" :: Array String }
author' = { "Name": "Phil", "Personal Interests": ["Functional Programming", "JavaScript"] }

> author'."Name"
"Phil"

> (author' { "Name" = "John" })."Name"
"John"
```

如果编译成 JavaScript，PureScript 编译器将允许你选择在 JavaScript 代码中编译成具有特殊意义的符号，比如 `__proto__`。

```javascript
oops = {__proto__: unsafeCoerce ""}.__proto__.constructor.constructor "alert('lol')" 0
-- When loaded onto a web page, this will display "lol" in a browser dialog box,
--   which is an effectful behavior despite this expression appearing to be pure.
```
