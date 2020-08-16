# Spago：从开始到结束

下面的示例使用 `spago` 作为构建工具和依赖项管理器。

## 创建项目

```bash
# 1. Sets up the initial files and structure of the project
spago init
```

如果你需要覆盖或添加包到标准包集中，可以使用 ``Configure the Package Set`。否则就使用 `Install Dependencies`。

### 配置包集

```bash
# 2a) Open the below file, read its top-level comment,
#       and follow its instructions to override/add packages
nano packages.dhall

# 2b) When finished, either verify that a single patched/added package
#       works with the rest of the set...
spago verify singlePackageName
#       or re-verify the entire package set
spago verify-set
```

### 锁定包集

注：Spago 现在会自动这样做。因此，人们可能不再需要这样做了。它是为了背景/历史的目的而提供的。

```bash
# 3) Freeze the package set to prevent security issues.
#     For a deeper explanation on what happens here,
#     see Dhall's safety guarantees wiki page:
#     https://github.com/dhall-lang/dhall-lang/wiki/Safety-guarantees
spago freeze
```

## 安装依赖

两种方式之一

```bash
# Install a package from the package set to your project
spago install packageName1 packageName2 # ...
```

## 编写代码

```bash
# Open the REPL to play with a few ideas or run simple tests
spago repl

# Build the docs
spago docs --open

# Automatically re-build project whenever a source/test file is changed/saved
# and clear the screen before rebuilding
spago build --watch --clear-screen
```

## 构建代码 

```bash
# Install all dependencies (if not done so already) and
# compile the code
spago build

# Build a developer-level executable file
spago bundle-app --main Module.Path.To.Main --to dist/index.js
node dist/index.js

# Build a production-level Node-backend file via Parcel
spago bundle-app --main Module.Path.To.Main --to dist/bundle-output.js
parcel build dist/bundle-output.js --target "node" -o app.js
```

要建立一个生产级的浏览器后端文件...

```bash
# Build a production-level browser-backend file
spago bundle-app --main Module.Path.To.Main --to dist/app.js
```

创建一个引用 "app.js "文件的HTML文件（dist/example.html）。

```html
<!DOCTYPE html>
<html lang="en" dir="ltr">
  <head>
    <meta charset="utf-8">
    <!-- Insert your title here -->
    <title>Some Title</title>
  </head>
  <body>
    <!-- Reference the outputted bundle here -->
    <script src="/app.js" charset="utf-8"></script>
  </body>
</html>
```

然后用 parcel 做 minification，打开生成的网页。

```bash
parcel build dist/example.html --target "browser" -o index.html --open
# it'll create a few files in the `dist/` folder and open the resulting
# "dist/index.html" file via your default web browser
```
