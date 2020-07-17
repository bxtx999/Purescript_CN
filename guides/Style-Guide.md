本文是编写 PureScript 库的指南。您可以将其视为一套最佳实践，或者如果您想为核心库做出贡献，则可以将其视为一套让您的拉取请求被接受的指南。

PureScript 核心库是如何将这些指南付诸实践的好例子。

## 构建工具

- 考虑使用一步到位的构建工具，如[Spago](https://github.com/purescript/spago)，让你的项目构建对其他用户来说尽可能简单。
- 在你的 `README` 文件中清楚地记录编译、测试和文档的构建步骤。

## 文档

- 尽量记录每一个导出的类型和函数。
- 即使一个函数看起来是“不言自明”的，也最好包含一个小注释或例子来帮助初学者。
- 简短的例子会有很大的帮助。试着将你的函数或类型的精髓提炼成几行，并将其作为Markdown代码块包含在你的文档中。

以前通常会将 `psc-docs` 生成的Markdown文档包含在资源库中。然而，现在我们有了 [Pursuit](http://pursuit.purescript.org/)，就不需要这样做了。推荐的方法是在 README 中链接到 Pursuit 上的文档，避免检查 `psc-docs` 生成的 Markdown 文档。

## 测试

- 在您的项目中包含测试，这样贡献者就可以轻松地验证他们的变化。

- PureScript有几个优秀的测试库，其中一些包括。

  + Spec: https://github.com/owickstrom/purescript-spec
  
  + Test-Unit：https://github.com/bodil/purescript-test-unit

## Examples

- 至少写一个例子，这可能是你的测试套件的一部分，来记录你的库如何被用来解决一些简单的完整的用例。
- 从您的 `README` 文件中清楚地链接到一个例子。
- 让它明显地说明如何运行你的例子。
- 如果你的例子产生了输出，考虑将输出包括在内（作为代码注释或在 `README`中）。

## 持续集成

- 为你的项目设置持续集成，以验证你的库是否正确编译。

- Travis可以很容易地进行设置。这里是一个模板 `.travis.yml`文件：

    ```yml
    language: node_js
    dist: trusty
    sudo: required
    node_js: 6
    install:
      - npm install purescript spago -g
    script:
      - spago build && spago test`
    ```

- 在你的 `README` 文件中显示Travis徽章，这样就可以看到构建状态。

## 发布

- 在[Bower](http://bower.io/search/?q=purescript)上分享您的库，将您的库名称前缀为 `purescript-` 这样其他人就可以找到您的作品。

    + Bower与git标签一起工作，但不需要任何现有的标签来发布。请运行`bower 0.0.0`版本，并为您的初始版本推送标签（这可以防止不打算发布的临时工作在`0.0.0` 库上泄露）。

    + 考虑到你可能需要实时编辑你的库，作为你的主项目开发的一部分，请查看`bower link`（在[这里](https://oncletom.io/2013/live-development-bower-component/)了解所有关于它的信息）。这将使你在工作时保持同步，并在准备好后方便发布。

    + 如果你打算为你的项目提供一个所需的软件包，那么`bower link` 也很有用。只需简单地将 repo 分叉，然后建立链接。一旦你有了额外的绑定，或者项目所需的功能，你可以通过一个 Pull Request 轻松地将它们贡献回源码仓库。

- 在[Pursuit](http://pursuit.purescript.org/)上分享您的库，这样开发人员就可以搜索您的库文档了。

    + 在你的 `README` 文件中加入 `Pursuit` 版本的徽章，并附上 `Pursuit`文档的链接。

## 类型

- 包括顶层声明的类型签名。必要时使用类型通配符来推断类型。

- 在适当的情况下，使用 `newtype` 来传递有关基元类型的附加信息（例如，考虑使用 `newtype EmailAddress` 而不是仅仅使用 `String`）。

- 在通过FFI提供绑定时，不要滥用类型系统。常见的滥用包括`forall props`、`{ | props }`(针对任何 Record)和 `forall a. a -> ...`(避免指定精确的类型)。PureScript拥有丰富的类型集，如`Foreign`和`FnX`，可以与JavaScript安全互操作，所以永远不需要这样做。如果需要，可以创建一个新的外来数据类型。

## 模块

- PureScript 遵循 Haskell 的模块命名空间约定。一些常见的顶层命名空间有
  + 数据结构及其功能的数据
  + 控制结构的控制
  + NodeJS相关功能的Node
- 在你的模块中加入模块导出列表。
- 尽量减少模块导出。

## 语言交互接口

- PureScript是一种新的语言，对高质量的FFI库有需求。
- 如果你已经写了一个有FFI代码到本地API的库，可以考虑把它拆成两个库，一个裸露的FFI绑定集，还有你额外的功能。

## 运行时需求

- NodeJS和web浏览器有区别。请在您的 `README` 文件中注意任何关于执行环境的假设（仅Node-only，仅浏览器等）。
- 你的FFI库可能带有运行时的依赖性。清楚地记录这些，以及任何安装步骤。

## 保持库文件最新

- 尽量保持你的库是最新的。
- 考虑使用 [VersionEye](https://www.versioneye.com/)。VersionEye 会读取你的 `bower.json` 文件，并在你的依赖关系过期时提醒你。如果你的 Travis 已经启动并运行了 CI，那么你就可以很容易地测试，看看这些变化是否会破坏变化，并更及时地解决它们。
- 如果你的库已经被废弃，请在 `README`  文件中清楚地记录这一事实。
