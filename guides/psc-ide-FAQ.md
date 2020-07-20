# `psc-ide` 常见问题

## 问：哪里能够下载 psc-ide？

答：从编译器版本 `>0.8.3` 起，`psc-ide` 将与编译器一起发布，并与编译器二进制文件一起安装。

## 问：如何让我的编辑器支持 psc-ide？

答：如果你使用 Atom、Emacs、Vim 或者 VS Code。

- Atom：

  + 安装 `language-purescript` 包
  + 安装 `ide-purescript` 包

- Emacs:
  
  从 MELPA 安装 `psc-ide`

- (Spacemacs):

  开启 PureScript layer

- Vim:

  `NeoBundle "frigoeu/psc-ide-vim"`

- VS Code：

  安装 `ide-purescript` 包

对于其他的编辑器，你可以自己编写插件！😄

## psc-ide 的文档在哪里？

答： https://github.com/purescript/purescript/tree/master/psc-ide

## 如何自动应用PureScript的建议？

答： 试试 https://github.com/nwolverson/purescript-suggest

# 故障排除

## 解析externs失败... ...

步骤1：删除 `output/`文件夹，重新编译项目
步骤2：检查 `psc --version` 和 `psc-ide-server --version` 是否匹配
步骤3：检查是否安装了旧版本 psc-ide (stack/npm/cabal)
步骤4：上IRC和@kritzcreek一起开始调试
