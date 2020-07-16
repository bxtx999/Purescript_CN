## 编译器

#### Atom

- [purescript-contrib/atom-language-purescript](https://github.com/purescript-contrib/atom-language-purescript)提供语法高亮显示

- [nwolverson/atom-ide-purescript](https://github.com/nwolverson/atom-ide-purescript)通过[psc-ide](https://github.com/purescript/purescript/tree/master/psc-ide)提供了构建支持、REPL和自动完成等功能。 

#### Emacs

安装这两个软件包并按照`psc-ide-emacs`自述文件中的说明进行操作。

- [purescript-mode](https://github.com/purescript-emacs/purescript-mode)语法突出显示和缩进(改编自haskell-mode)

- [psc-ide-emacs](https://github.com/purescript-emacs/psc-ide-emacs)Emacs插件，显示了对编译器IDE的支持

通过comint支持PSCI：

- [psci-mode](https://github.com/purescript-emacs/emacs-psci)是REPL次要模式

Spacemacs用户可以只使用[`purescript`层](https://github.com/syl20bnr/spacemacs/tree/master/layers/%2Blang/purescript)。

#### Sublime Text 2

- [pureScript包](https://sublime.wbond.net/search/PureScript) by joneshf

#### Vim

- [purescript-vim](https://github.com/raichoo/purescript-vim)语法突出显示和缩进

- [FrigoEU/psc-ide-vim](https://github.com/FrigoEU/psc-ide-vim/)

- [w0rp/ale](https://github.com/w0rp/ale)支持[purescript-language-server](https://github.com/nwolverson/purescript-language-server)

#### VS代码

- [nwolverson / vscode-ide-purescript](https://github.com/nwolverson/vscode-ide-purescript)

#### 通用

- 要生成`TAGS`文件，使用`purs docs --format etags`(或`--format ctags`)

## 构建工具和包管理器

保持：

- [spago](https://github.com/purescript/spago) - 由[Dhall](https://github.com/dhall-lang/dhall-lang)和[package-set](https://github.com/purescript/package-sets)

- [psc-package](https://github.com/purescript/psc-package) - 基于软件包集概念的PureScript软件包管理器

- [purs-loader](https://github.com/ethul/purs-loader/) - 用于WebPack的PureScript加载器

- [pscid](https://github.com/kRITZCREEK/pscid)是用于PS项目的轻量级文件监视程序/测试运行器，以与编辑器无关的方式提供即时重建

- [purescript-psa](https://github.com/natefaubion/purescript-psa) - 适用于`psc'的漂亮，灵活的错误/警告报告前端，具有颜色，错误中的原始源范围，警告过滤和持久性。

- [Pulp](https://github.com/purescript-contrib/pulp) - PureScript的独立构建系统(`ppm中的[pulp](https://www.npmjs.com/package/pulp)) 。 *推荐西班牙浆优先于纸浆。*

- [Gulp任务](https://github.com/purescript-contrib/gulp-purescript)(`npm`中的`gulp-purescript`)。 *建议在PureScript Gulp任务上使用Spago。*

不再维护：

- [psvm-js](https://github.com/ThomasCrvsr/psvm-js) - PureScript版本管理器

- [psc-pane](https://github.com/anttih/psc-pane) - 自动重新加载编译器，可格式化单个错误以适合窗口