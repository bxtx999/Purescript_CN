# PureScript 与 Haskell 的区别

## 评估策略

与 Haskell 不同，PureScript 采用严格的评估策略。

由于评估策略与 JavaScript 相匹配，因此与现有代码的互操作性是微不足道的-——从 PureScript 模块中导出的函数与任何“普通” JavaScript 函数的行为完全一样，相应地，通过 FFI 调用 JavaScript 也很简单。

保持严格的评估也意味着不需要运行时系统或过于复杂的 JavaScript 输出。在需要的时候也应该可以写出更高性能的代码，因为在 JavaScript 之上引入懒惰会带来不可避免的开销。

## Prelude/base

## 模块的导入与导出

## 类型

### 显示 forall

### 数字类型

### `Uint`

### `[a]`

## Record

## 类型类

### 箭头方向

### 命名实例

### 派生

### 孤儿实例

### 默认成员

### 类型类分级

## 元组

## 合成算子

## `return`

## 列表推导

## `$` 的非特殊处理

## 定义算子

## 算子 section

## 扩展

## `error` 和 `undefined`

## 文档注释

## Haskell 一些代码在哪里
