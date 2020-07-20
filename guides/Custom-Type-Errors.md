查看 [24 days of PureScript 2016 post](https://github.com/paf31/24-days-of-purescript-2016/blob/master/21.markdown)

## `Warn`类型类

在 `Prim` 中，有一个 `Warn` 的类型类,它和 `Fail` 一样是以 `Symbol` 为索引的。当编译器求解 `Warn` 约束时，它将 trivially 求解实例，并将消息打印出来作为用户定义的警告。

### 弃用实例

一个可能的用例是自定义的废弃警告。从一个导出函数 `notBad` 的库开始:

```purescript
notBad :: Int
notBad = 21
```

但现在你决定有更好的东西；你想弃用 `notBad` 而使用这个新功能。

```purescript
notBad :: Warn "`notBad` is deprecated. Prefer `better` instead." => Int
notBad = 21

better :: Int
better = 42
```

现在当有人使用 `notBad` 时，会看到以下编译器警告。

```purescript
  A custom warning occurred while solving type class constraints:

    `notBad` is deprecated. Prefer `better` instead.
```
