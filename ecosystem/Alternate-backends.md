对于`psc`的默认JavaScript后端，有多种替代方案：

## 主动维护

| 源码                                                                          | PS 版本 | 目标        | 状态   | 备注               | 
|:--------------------------------------------------------------------------------------|:-----------|:--------------|:------|:-----------------------|
| [andyarvanitis/purescript-native](https://github.com/andyarvanitis/purescript-native)             | 0.13.x    | C++11 或 Go         | active | all applicable tests in `purescript/tests/purs/passing` are passing |
| [purerl/purerl](https://github.com/purerl/purerl) | 0.13.5 | Erlang | active |

## 测试中

| 源码                                                                                   | PS 版本 | 目标        | 
|:----------------------------------------------------------------------------------------------|:-----------|:--------------|
| [andyarvanitis/purescript-clojure](https://github.com/andyarvanitis/purescript-clojure)       |            | Clojure (JVM) |
| [csicar/pskt](https://github.com/csicar/pskt)                                                 |            | Kotlin        |
| [pure-c/pure-c](https://github.com/pure-c/pure-c)                                             |            | C (Clang)     |
| [purescript-python/purescript-python](https://github.com/purescript-python/purescript-python) | 0.13.x     | Python >= 3.5 |
| [epost/psc-query](https://github.com/epost/psc-query)                                         | 0.11.7     | Datalog       |
| [paulyoung/pureswift](https://github.com/paulyoung/pureswift)                                 |            | Swift         |

## 不再维护

| 源码                                                                           | PS 版本 | 目标        |
|:--------------------------------------------------------------------------------------|:-----------|:--------------|
| [lua-purescript/purescript](https://github.com/lua-purescript/purescript)             | 0.9.1.0    | Lua           |
| [slamdata/truffled-purescript](https://github.com/slamdata/truffled-purescript)       | 0.7.5.x    | Truffle (JVM) |
| [osa1/psc-lua](https://github.com/osa1/psc-lua)                                       | 0.5.x      | Lua           |
| [Gabriel439/Purescript-to-Python](https://github.com/Gabriel439/Purescript-to-Python) |            | Python        |
| [PyreScript](https://github.com/joneshf/pyrescript)                                   | 0.9.1      | Python        |
| [tfausak/thran](https://github.com/tfausak/thran)                                     | 0.11.6     | Haskell (GHC) |