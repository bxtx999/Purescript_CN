## 评估策略

与 Haskell 不同，PureScript 采用严格的评估策略。

由于评估策略与 JavaScript 相匹配，因此与现有代码的互操作性是微不足道的-——从 PureScript 模块中导出的函数与任何“普通” JavaScript 函数的行为完全一样，相应地，通过 FFI 调用 JavaScript 也很简单。

保持严格的评估也意味着不需要运行时系统或过于复杂的 JavaScript 输出。在需要的时候也应该可以写出更高性能的代码，因为在 JavaScript 之上引入懒惰会带来不可避免的开销。

## Prelude/base

