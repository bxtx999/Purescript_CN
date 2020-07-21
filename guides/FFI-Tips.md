# PureScript FFI 小技巧

## 使用 `mkFn` 和 `runFn`

```purescript
module Path where

foreign import joinPath :: FilePath -> FilePath -> FilePath
```

```javascript
exports.joinPath = function(start) {
  return function(end) {
    return require('path').join(start, end);
  };
};
```

```purescript
module Path where

foreign import joinPathImpl :: Fn2 FilePath FilePath FilePath
```

```javascript
exports.joinPathImpl = require('path').join;
```

```purescript
foreign import joinPathImpl :: Fn2 FilePath FilePath FilePath

joinPath :: FilePath -> FilePath -> FilePath
joinPath = runFn2 joinPathImpl
```

```purescript
joinPath :: FilePath -> FilePath -> FilePath
joinPath start end = runFn2 joinPathImpl start end
```

## 避免直接从 foreign module 调用 PS 代码

```purescript
foreign import doSomethingImpl :: forall a. Fn2 (a -> Boolean) a (Maybe a)

doSomething :: forall a. (a -> Boolean) -> a -> Maybe a
doSomething fn x = runFn2 doSomethingImpl fn x
```

```javascript
exports.doSomethingImpl = function(fn, x) {
  if (fn(x)) {
    return Data_Maybe.Just.create(x);
  } else {
    return Data_Maybe.Nothing.value;
  }
};
```

```purescript
foreign import doSomethingImpl :: forall a. Fn4 (a -> Maybe a) (Maybe a) (a -> Boolean) a (Maybe a)

doSomething :: forall a. (a -> Boolean) -> a -> Maybe a
doSomething fn x = runFn4 doSomethingImpl Just Nothing fn x
```

```javascript
exports.doSomethingImpl = function(just, nothing, fn, value) {
  if (fn(value)) {
    return just(value);
  } else {
    return nothing;
  }
};
```

```purescript
foreign import showSomethingImpl :: forall a. Fn3 (Maybe a -> Boolean) (a -> String) (Maybe a) String

showSomething :: forall a. (Show a) => Maybe a -> String
showSomething x = runFn3 showSomethingImpl isJust show x
```

```javascript
exports.doSomethingImpl = function(isJust, show, value) {
  if (isJust(value)) {
    return "It's something: " + show(value);
  } else {
    return "It's nothing.";
  }
};
```

## 为什么我的 `Eff` 传递到一个普通的 JS 函数时不工作？

'*PureScript by Example*' 中 <[Representing Side Effects](https://book.purescript.org/chapter10.html#representing-side-effects)>

为了避免过早地评估效果（或评估根本不应该评估的效果），PureScript 将它们包装在常量函数中。

```javascript
exports.myEff = function() {
  return doSomethingEffectful(1, 2, 3);
}
```

```purescript
foreign import myEff :: forall eff. Eff (myEff :: MYEFF | eff) SomeType
```