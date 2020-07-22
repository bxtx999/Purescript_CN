
外部函数接口 (FFI) 允许您与 JavaScript 编写的代码交互(或其他后端语言)。

## 引入值

`foreign import` 关键字声明定义在 JavaScript 中的值与类型。

```purescript
-- src/Math.purs
module Math where
foreign import pow :: Number -> Number -> Number
```

从 FFI 导入值时，值本身是在单独的文件中定义的。文件名应与 PureScript 源文件名相同，只是根据后端语言情况用 `.purs` 代替文件扩展名。例如，如果上面的代码是在 `src/Math.purs` 中写的，那么相应的 FFI 文件就会在 `src/Math.js` 中，它可能看起来像这样：

```javascript
// src/Math.js
"use strict";
exports.pow = function(x) {
  return function(y) {
    return Math.pow(x,y);
  };
};
```

编译器会检查你的 FFI 文件是否导出了你通过 `foreign import` 引入的任何值。然而，编译器不能检查 FFI 中定义的值在运行时表示的类型是否正确。你有责任确保类型正确，如果你声明你导入的值是一个 `Int`，这个值实际上就应该是一个 `Int` 类型（而不是一个数组、函数或其他任何类型）。

## 引入类型

要声明一个新的抽象类型（没有构造函数），使用 `foreign import data` 并提供 kind。

```purescript
foreign import data DOMElement :: Type

foreign import document :: {
  createElement :: String -> DOMElement
}
```

当用这种方式声明类型时，你应声明类型为任何 kind，而不只使用 `Type`。例如，声明 effect 的 row 类型。

```purescript
foreign import data MyRow :: # Effect
```
