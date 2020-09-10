# 为什么学习 PureScript

所有语言都在各个领域和各个方面进行权衡：

- 学习曲线
- 抽象
- 语法
- 错误
- 类型系统
- 等等

## 为什么使用 JavaScript 来构建程序

## 为什么不写 JavaScript 来构建程序

## 为什么使用 PureScript 来写程序

**TL;DR**

- [The Power of Composition](https://youtu.be/vDe-4o8Uwl8?t=8)
- [Purescript: Tomorrow's Javascript Today](https://www.youtube.com/watch?time_continue=22&v=5AtyWgQ3vv0)
- [Code Reuse in PureScript: Functions, Type Classes, and Interpreters](https://youtu.be/GlUcCPmH8wI?t=24)
- [Phil Freeman's post: 'Why You Should Use PureScript'](https://gist.github.com/paf31/adfd15fbb1ac8b99fc68be2c9aca8427)
- PureScript's "Real World App"s
  - See the [Halogen version of 'Real World App'](https://github.com/thomashoneyman/purescript-halogen-realworld)
  - See the [React version of 'Real World App'](https://github.com/jonasbuntinx/purescript-react-realworld)
- [A Discourse pose describing some of the disadvantages of TypeScript and Elmm when compared to PureScript](https://discourse.purescript.org/t/language-highlights/1471)

对于那些说“ PureScript 死了，不好，愚蠢，不值得学习等”的人。 针对这些情况，建议您观看[开源的硬部分](https://www.youtube.com/watch?v=o_4EX4dPppA)

### 语言比较

#### PureScript 与 TypeScript

#### PureScript 与 Elm

#### PureScript 与 GHCJS

### PureScript 的优势

#### 强烈遵守函数式编程范例

- [A Secret Weapon for Startups -- Functional Programming?](https://www.ramanan.com/personal-blog/2019/2/25/functional-programming-and-venture-capital)
- Paradigm shifts, such as the one demonstrated by this video using C++, are what enable programs with less problems: [Logging a function's name each time it is called: migrating an "object-oriented paradigm" solution to an "functional paradigm" solution](https://www.youtube.com/embed/i9CU4CuHADQ?start=540). As will be explained later, this is what is known as the "Writer Monad."
- [Object-oriented "design patterns" in FP languages](http://blog.ezyang.com/2010/05/design-patterns-in-haskel/) are often just functions in disguise. Rather than learning the 20 different design patterns, one can learn how functions work and can be used to create really beautiful concepts and solutions.
- [Functional Architecture: The Pits of Success](https://www.youtube.com/watch?v=US8QG9I1XW0). To summarize this video, FP languages force you to structure your code in a way that makes it:
  - easy to test in an unbiased way (Can I prove that the logic/algorithm that solves the business problem is correct and works according to the specification despite any programmer's laziness or lack of foresight in thinking of a possible scenario where the code could fail?)
  - easy to add/change/remove a "backend" to account for trends, new insights, or faster code (Without introducing a new bug or deleting a current feature, can I switch from Company A's database to Company B's database without rewriting more than 30 lines of code?)
  - unconcerning to allow a new developer to work on the code, knowing that he/she cannot screw up anything major (Can the Lead/Senior Developer take the weekend off and return, knowing that it's extraordinarily difficult for developers with little experience to break something?)

#### 强大的静态类型系统

- This video explains how a type system with `algebraic data types` comes with a number of benefits (note: it uses a different syntax than PureScript: [Domain Modeling Made Functional](https://www.youtube.com/watch?v=Up7LcbGZFuo). To summarize it, `algebraic data types`
  - allow you to model a domain at a 1-to-1 ratio
  - make impossible states impossible
  - become your always-up-to-date UML diagrams
  - make it easy for new developers to learn how the code is structured
  - guide how business logic should be implemented
- The PureScript compiler infers most of your types for you. For those who are curious and want to understand how that works, see this video: [Type Inference From Scratch](https://www.youtube.com/watch?v=ytPAlhnAKro)
- The compiler (via its warning and error messages) is your friend, not your enemy. I was not able to find a good concise explanation, but here's a few reasons why. It
  - prevents you from releasing bug-filled code to a customer. (Can I guarantee that the code "just works" or cannot be built at all?)
  - forces you to handle most errors correctly the first time rather than permit you to throw them under the rug because you are lazy or foolish (Can I guarantee all possible errors will not create future problems that lead to short-term hard-to-understand code that rarely gets cleaned up and ultimately costs the company more time to fix than if it had just been written correctly the first time?)
  - helps you figure out which type to use when the types get complicated (explained later in this repository: `Hello World/Debugging/`)
- This video explains how a type system with `type classes` allow one to re-use "dumb old data structures" (i.e. `algebraic data types`) rather than create many new data structures that differ only one slight way: [Type Classes vs the World](https://www.youtube.com/watch?v=hIZxTQP1ifo). To summarize it, `type classes`
  - allow you to write declarative code ("this is what will be true") rather than imperative code ("this is how to make truth true (hopefully, you got it right)")
  - enables the compiler to infer runtime code

#### 默认不变的持久数据结构

TODO，但总体思路是：

-不可变的数据结构是默认结构，并且始终可以正常工作（与某些其他语言不同）

-可变数据结构是可选的

-这样的数据结构更易于使用和推理，因为它们不会改变

#### 支持多后端的简单 FFI

TODO，但总体思路是：

- [PureScript compiles to other languages besides JavaScript](https://github.com/purescript/documentation/blob/master/ecosystem/Alternate-backends.md). 因此，用PureScript编写一个库可以在多种语言中工作，并且可以选择最能解决问题的后端。
- 可以轻松地以模块化，逐段的方式从其他某种语言或框架（例如TypeScript，Angular等）迁移到PureScript

## FAQ：回答人们可能遇到的各种问题

### 当从语言 X 迁移到 PureScript 时，开发团队通常会遇到什么问题，这些问题有多难克服

- 看 Phil Freeman 自己的博客: [PureScript and Haskell at Lumi](https://medium.com/fuzzy-sharp/purescript-and-haskell-at-lumi-7e8e2b16fb13)
- Thomas Honeyman 的 [How to Replace React Components with PureScript's React libraries](https://thomashoneyman.com/articles/replace-react-components-with-purescript/)
- [(Video) JavaScript to PureScript - a Migration Story](https://www.youtube.com/watch?v=bt130Z7UNPE) & [Slides](https://github.com/lambdaconf/lambdaconf-2018/blob/master/LC18-slides/Curran-angular-purescript-halogen.pdf)
- [(Video) Adopting Pure FP Incrementally - Engineering at Lumi](https://www.youtube.com/watch?v=SiGXTcFEvHo)
- 看 [Introducing Haskell to a Company](https://alasconnect.github.io/blog/posts/2018-10-02-introducing-haskell-to-a-company.html) 同样适用于 PureScript。
- 看 [Robert Kluin - Introducing A Functional Language At Work - λC 2018](https://www.youtube.com/watch?v=3USNLflRRUA)/[slides](https://github.com/lambdaconf/lambdaconf-2018/blob/master/LC18-slides/A%20Developer%E2%80%99s%20Guide%20to%20Introducing%20A%20Functional%20Language%20At%20Work.pdf) 当他试图在工作中引入Scala时，他讲述了一个关于“什么会出错以及为什么会出错”的警示故事。
