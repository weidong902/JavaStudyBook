# 用 GitBook 创建一本书

Gitbook 首先是一个软件，它使用 Git 和 Markdown 来编排书本，如果你没有听过 Git 和 Markdown，那么 gitbook 可能不适合你直接入手，你需要先去学习 Git 和 Markdown。Git 是一个版本控制工具，Markdown 是一个文本编辑语法，基本的使用大概几个小时就可以都学会了，并不复杂。

这是 Gitbook 项目主页上对 Gitbook 的定义。

> Modern book format and toolchain using Git and Markdown .

# 安装 GitBook

首先需要安装==nodejs==，以便能够使用 npm 来安装 gitbook

```
$ wget -qO- https://raw.github.com/creationix/nvm/v0.33.11/install.sh | sh
```

该命令会安装 nvm 命令，安装好以后会添加 nvm 的环境变量到 `.bashrc` 文件里，接下来退出终端，重新登陆使得 `.bashrc`文件生效，或者使用其他方法使添加的环境变量生效。

接下来使用下列命令来安装 npm

```
$ nvm install stable
```

可以使用下列命令来查看 npm 的版本，比如我这次安装的是 6.11.3 版本。

```
[erdong@testhost ~]$ npm --version
6.11.3
[erdong@testhost ~]$
```

接下来使用 npm 来安装 gitbook（==npm install gitbook-cli -g==）

```
$ npm install gitbook-cli -g
```

# 创建第一本书

首先新建一个目录，这个目录就是这本书的一个载体，就好比在现实世界里你要先准备一个本子，以后所有的内容会写在这个本子上。在 gitbook，我们以后所有的内容都会存放在这个目录里。

```
$ mkdir weidong-first-book
```

接下来我们初始化这个目录，就好比你在你准备的本子上做一些基本的工作，

```
[erdong@testhost ~]$ cd weidong-first-book
[erdong@testhost erdong-first-book]$ gitbook init
[erdong@testhost erdong-first-book]$ ls
README.md  SUMMARY.md
```

初始化过后，在这个文件夹会生成 2 个文件，`README.md` 和 `SUMMARY.md` 。

- README.md ： 是书的简单介绍，类似于一本书的序或者前言部分。
- SUMMARY.md ： 是书的目录，程序按照这个文件来生成书的结构。

初始化结束后，就可以写东西了，

比如我们现在往这两个文件里写入以下内容，在 README.md 文件中我们写入如下内容：

```
# Introduction

This erdong's first book

This a samples in GitHub
```

在 SUMMARY.md 文件中写入如下内容：

```
# Summary

* [Introduction](README.md)

* [Part I]()
* [Part II]()
* [Part III]()
```

写入内容后，我们可以通过 `gitbook serve` 命令来预览我们写的书（==gitbook serve==）

```
[erdong@testhost erdong-first-book]$ gitbook serve
Live reload server started on port: 35729
Press CTRL+C to quit ...

info: 7 plugins are installed
info: loading plugin "livereload"... OK
info: loading plugin "highlight"... OK
info: loading plugin "search"... OK
info: loading plugin "lunr"... OK
info: loading plugin "sharing"... OK
info: loading plugin "fontsettings"... OK
info: loading plugin "theme-default"... OK
info: found 1 pages
info: found 0 asset files
info: >> generation finished with success in 0.5s !

Starting server ...
Serving book on http://localhost:4000
```

这样会在前台运行一个 web 程序，对外以 4000 端口提供一个可以浏览的页面，浏览的地址是 [http://localhost:4000](http://localhost:4000/) ，

这样我们就可以查看我们写好的书了。如下图所示。

![Gitbook Introduction](https://image.erdong.site/blog/2019/10/16/gitbookintroduction.jpg)

如果我们不想使用这种方式来预览书籍的话，也可以用 gitbook 生成一份静态的页面，放在 web 服务器或者其他可以提供 web 浏览访问的地方。生成静态页面使用 `gitbook build` 命令即可，会生成一个 `_book` 的文件夹，这个目录里放的就是生成好的静态页面，拷贝到对应的目录即可。

```
[erdong@testhost erdong-first-book]$ gitbook build 
info: 7 plugins are installed
info: 6 explicitly listed
info: loading plugin "highlight"... OK
info: loading plugin "search"... OK
info: loading plugin "lunr"... OK
info: loading plugin "sharing"... OK
info: loading plugin "fontsettings"... OK
info: loading plugin "theme-default"... OK
info: found 1 pages
info: found 0 asset files
info: >> generation finished with success in 0.5s !
[erdong@testhost erdong-first-book]$ 
```

# 小结

这样，我们就通过 GitBook 工具写了一本书，或者一个小册子。