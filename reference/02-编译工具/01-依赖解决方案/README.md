# 依赖解决方案

有两种依赖性管理的解决方案，每一种都有它们自己的 "目标受众"。

- Bower (库开发人员)
- Spago (程序开发人员)

社区需要这两种解决方案，原因将在后面解释。

每一种解决方案都将在自己的文件中进一步解释。然而，我们可以用一个 "粗略的名字 "来描述它们：

- 典型的包管理器 (Bower)
- 重要的 `git clone` 工具 (Spago)

## 依赖管理库的比较

这种横向比较应该被认为是“天壤之别”。

| | Bower | Spago |
| - | - | - |
| 理想用户 | <ul><li>库开发人员</li><li>探索语言、生态的人</li></ol> | <ul><li>应用程序开发人员</li><li>通过经 CI 验证的学习材料学习语言的人。</li></ul> |
| 正式/非正式 | Official<br>(parts of the PureScript compiler depend on it) | Unofficial
| Trajectory | Moving away? | Moving towards?
| 设计目标 | ? | [See this summary of a project that later "evolved" into Spago](https://github.com/purescript/psc-package#design-goals)
| 优点 | <ul><li>**Bower is required for one to publish a library's docs to [Pursuit](https://pursuit.purescript.org/)**</li><li>Bower caches its dependencies, so you can download a version of a package only once and use it everywhere on your system</li><li>It's the "official" dependency manager for Purescript</li></ul> | <ul><li>**One never experiences the aforementioned "transitive dependency conflicts"**</li><li>Uses Git to work, which is usually already installed on all systems</li><li>Reliably produces reproducible Purescript dependencies</li><li>Reduces verbosity somewhat by omitting the `purescript-` prefix on PureScript packages</li></ul>
| 缺点 | <ul><li>**When a recent compiler release has been made, one will encounter the 'transitive dependency' issue above quite frequently until all the libraries in the ecosystem are updated to the new release. Figuring out which version of a package works with another wastes a lot of time.**</li><li>Uses node to work, which may not be installed on all systems</li><li>Doesn't always produce reproducible dependencies due to its caching feature.</li></ul> | <ul><li>**One cannot publish a library's docs to Pursuit.**</li><li>Uses more storage space due to duplicating code when one has multiple projects across their system.</li></ul>