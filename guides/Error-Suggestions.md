# Error/warning 建议

一些编译器错误/警告提供了一个如何修复问题的建议，不仅在编译器的文本输出中，而且在机器可使用的JSON格式输出中。

### 推荐工具


* [purescript-suggest](https://github.com/nwolverson/purescript-suggest) - 命令行工具，可以显示并应用所有建议
* [pscid](https://github.com/kRITZCREEK/pscid#suggestions) - 应用单一建议
* [psc-ide-emacs](https://github.com/epost/psc-ide-emacs#insert-suggestion-from-error-c-c-m-s)
* [psc-ide-vim](https://github.com/FrigoEU/psc-ide-vim/blob/master/doc/psc-ide-vim.txt#L61)
* [atom-ide-purescript](https://github.com/nwolverson/atom-ide-purescript#error-suggestions--quick-fix)
* [vscode-ide-purescript](https://github.com/nwolverson/vscode-ide-purescript)

### 可修复问题

不同种类问题的自动修复建议：

- 引入错误(Import warnings)

   例如，删除未使用的引用，显性引用

- 缺少类型注解(Missing type annotation)

   顶层类型注解和 `_`
