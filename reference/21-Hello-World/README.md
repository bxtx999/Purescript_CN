# Hello World

这个文件夹将记录在 PureScript 中创建一个简单的基于控制台的程序所需的所有内容。

这将解释：

- FP 编程的哲学基础
- Prelude 库(包括 Functor、Apply、Applicative、Bind 和 Monad 解释)
- 一个简单的 "Hello World" 程序和其他效果。
- 自定义编译器警告/错误
- 本地可变状态与全局可变状态的区别
- 如何测试代码
- 如何进行基准/配置文件代码
- 如何构建一个 FP 应用
- 各种类型级编程库的概述
- 用 PureScript 写的几个基于控制台的游戏（凑合着玩）

为了实现这些目标，本文将讲述以下库：

- Basic
  - Prelude
  - Prim.TypeError
- Effects
  - Effect
  - Console
  - Random
  - Aff
- State
  - ST
  - Refs
- Testing
  - Spec
  - Quick Check
  - Quick Check Laws
- Benchmarking
  - Benchotron
- Advanced
  - Variant/VariantF
  - MTL
  - Free
  - Run
- UIs
  - Node ReadLine
  - Halogen

## 必要链接

[Pursuit - Docs for Purescript packages](http://pursuit.purescript.org/)

## 其他学习资源

在理解函数式编程方面我们除了这个 repo 还有一些其他选择。（这些选择不一定是 "要么X要么Y要么Z"，可以是 "X辅以Y，再加一点Z"）

### PureScript

- `Purescript By Example`。(参见 `ROOT_FOLDER/Getting Started/Other Important Info.md` 来获取相关链接和说明)
- [Purescript Resources](https://purescript-resources.readthedocs.io/en/latest/index.htmlv) —— Justin Woo 的 Read the Docs(RTD) 作品。

### JavaScript

[Make the Leap from JavaScript to PureScript](https://medium.com/@kelleyalex/index-make-the-leap-from-javascript-to-purescript-a1566d657e9c)

### Haskell

由于 Purescript 的设计深受 Haskell 的启发，并且与 Haskell 非常相似，因此可以通过这些 Haskell 学习资源来学习 Purescript 的很多知识。注意：Haskell的名称和类型类并不总是与Purescript版本相对应。

| 行动 | 优点 | 缺点 |
| --- | --- | --- |
| Read the documentation and source code for a type class and a few data types' implementations of said type classes | Free | Takes a lot of time; requires intuition to understand type class' usefulness / relation to others.
| Read through the articles on or pay for training from [FP Complete's opinionated Haskell website](https://haskell.fpcomplete.com/) | Free / Paid | (Haven't done it so I don't know)
| Read through the intermediate-level Haskell articles in the [`Applied Haskell 2018` GitHub Repo](https://github.com/fpco/applied-haskell#readme) | Free | (Haven't done it so I don't know)
| Read through some of the free course materials taught by someone well informed about Haskell [here (you'll need to scroll towards the bottom)](https://github.com/bitemyapp/learnhaskell) | Free; more principled explanations | Looking at just slides without hearing someone teach using them is not usually as clear as when someone does teach using them or reading through a textbook on the same matter.
| Read through the extremely lengthy ["What I wish I knew when learning Haskell"](http://dev.stephendiehl.com/hask/) site | Free; provides a better overview of basic to advanced topics | Very long; not necessarily deep and clear in its explanations
| Read and do the exercises from [The Haskell Book](http://haskellbook.com/) | The "standard" for teaching Haskell and FP concepts in general: good explanations; good exercises; teaches "programming in the small" | Costs money; costs time; the exercises will stretch you
| Read and do the exercises from [Haskell Cookbook](https://leanpub.com/haskellcookbook), and then its follow up book [Haskell Cookbook 2](https://leanpub.com/haskellcookbook2) | Free/Cheap; simpler than the Haskell book; gets to ideas faster; teaches "programming in the large" | May be harder for a new beginner (I haven't read it yet)
| Watch the Intro to FP course on edX.org [here](https://www.edx.org/course/introduction-functional-programming-delftx-fp101x-0) | Free (or paid) | (Haven't done it so I don't know)
| Read the relevant chapters from [Learn You a Haskell for Great Good](http://learnyouahaskell.com/chapters) | Free | I read elsewhere that it's "outdated". See [this Reddit comment's warning about learning from LYAHH](https://www.reddit.com/r/programming/comments/50m99l/new_edition_of_programming_in_haskell_now/d75pyxf/)

### 其他链接

- [LambdaConf YouTube Channel](https://www.youtube.com/channel/UCEtohQeDqMSebi2yvLMUItg)
