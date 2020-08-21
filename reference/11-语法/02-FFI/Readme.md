# 外部函数语法 (FFI) 语法

## 不同的后端

除了编译为 JavaScript 外，Purescript 还可以编译为其他语言。 请参考[此链接](https://github.com/purescript/documentation/blob/master/ecosystem/Alternate-backends.md)以获取完整列表（可能已过时）。

## 语法

此文件夹提供了一些关于 JavaScript 后端简单情况的 FFI 示例。但是，关于如何正确地执行 FFI，请参见为 [PureScript 包装 JavaScript](https://blog.ndk.io/purescript-ffi.html) 来获得更详细的示例。

您还应该查看一下 PureScript 和 JavaScript 源代码的 `Effect.Uncurried`：

- [Purescript](https://github.com/purescript/purescript-effect/blob/v2.0.0/src/Effect/Uncurried.purs#L139)
- [Javascript](https://github.com/purescript/purescript-effect/blob/v2.0.0/src/Effect/Uncurried.js#L1)

最后在某些情况下，可能需要使用 `Effect` 来编写 FFI，但是 `Effect` 并不是最好的类型。在这种情况下，看看 [Aff 的 FFI](https://pursuit.purescript.org/packages/purescript-aff/5.0.2/docs/Effect.Aff.Compat)：

## 在 PureScript 中使用 JavaScript 上下文

> If I want to create some standalone PureScript functions that integrate with a broader JavaScript codebase, is there a way to take the output from browserify and accomplish that?

```bash
$ cat src/GCD.purs
module GCD where

import Prelude

gcd2 :: Int -> Int -> Int
gcd2 n m | n == 0 = m
gcd2 n m | m == 0 = n
gcd2 n m | n > m = gcd (n - m) m
gcd2 n m = gcd (m - n) n

$ cat build.sh
pulp browserify --main GCD --skip-entry-point --standalone index --to js/index.js```
```

> As-in, is there some magic required to import `index.js` and expose `gcd2`?

> At Awake, we would write something like a `GCD.Interop`, which has bindings from JS reps to the PS reps, and then we would just import the output directly

> Can you give an example of what that would look like specifically?

> Sure.

```purescript
module GCD.Interop where
import Prelude
import GCD as GCD
import Data.Function.Uncurried (mkFn2)

gcd2 :: Fn2 Int Int Int
gcd2 = mkFn2 GCD.gcd2```
```

> That gets compiled to `./output/GCD.Interop/index.js` and then in a js file I can do

```javascript
import { gcd2 } from "./output/GCD.Interop/index.js";
gcd2(4, 5);
```

> Okay cool. That is helpful. Thank you!

> You can import the original file directly as well but the reason we write an Interop file is so we can change PS files without breaking JS consumers, or at least, if we change types, we know that JS consumers need to be verified

> If I load the original file via a script tag, I can't figure out how to call `gcd2`, as it feels like it's hidden behind a closure, am I wrong? I don't totally understand the magic browserify is doing...

> If you use browserify with standalone, then you can configure what the global variable is. So `pulp browserify --main GCD --skip-entry-point --standalone GCD --to index.js`. If you load that index file with a script tag, then you’ll have `GCD.gcd2` available in the global namesapce

> That's what I was looking for.
