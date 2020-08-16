# Bower：从开始到结束

**警告**：此代码尚未检查。 大部分应该是正确的，但是某些部分可能是错误的。

## 创建项目

其中的一种方法：

```bash
pulp init
```

## 安装依赖

```bash
# Need to install NPM packages and initialize them
npm install npm-package1 npm-package2
npm install
bower install package1 package2 --save
bower install
```

### 写代码

```bash
# Open the REPL to play with a few ideas or run simple tests
pulp repl

# Automatically re-build project whenever a source file is changed/saved
pulp --watch --before 'clear' build

# Automatically re-test project whenever a source/test file is changed/saved
pulp --watch --before 'clear' test

# Build a developer version
pulp browserify --to dist/fileName.js # if program
pulp browserify --no-main-check --to dist/fileName.js # if library

# Run the program and pass args to the underlying program
pulp run -- arg1PassedToProgram arg2PassedToProgram
```

### 第一次发布包

参见 Pursuit 上[作者的帮助页面](https://pursuit.purescript.org/help/authors)。它的指示比下面的更有权威性。

```bash
# Build the docs
pulp docs -- --format html
# Then read over them to insure there aren't any formatting issues or typos

# Make a production build via Browserify
#   If building a program...
pulp browserify --optimise --to dist/fileName.js
#   If building a library
pulp browserify --no-main-check --optimise --to dist/fileName.js

# Set the initial version
pulp version v0.1.0

# Publish the version
pulp publish
```

### 发布已经发布的包的新版本

```bash
# Build and check the docs
pulp docs -- --format html

# Make a production build
#   If building a program...
pulp browserify --optimise --to dist/fileName.js
#   If building a library
pulp browserify --no-main-check --optimise --to dist/fileName.js

# bump project version
pulp version major
pulp version minor
pulp version patch
# or specify a version
pulp version v1.5.0

# publish it
pulp publish
```
