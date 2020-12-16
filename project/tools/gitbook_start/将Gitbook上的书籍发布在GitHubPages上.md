# 将Gitbook上的书籍发布在GitHubPages上

GitBook 是一个基于 Node.js 的命令行工具，可使用 Markdown 来制作精美的电子书. 

但是Gitbook由于网络问题,许多其他人发布的项目不能直接查看,或者保存.但是我们可以通过Github将fork Gitbook 源码到自己的Github,并设置Github Pages 页面访问, 来实现保存Gitbook项目的目的.

GitHub Pages 简单说就是一个可以托管静态网站的 Git 项目，支持使用 markdown 语法以及 Jekyll 来构建，或者直接使用已经生成好的静态站点。我们就可以使用GitPages搭建自己的个人博客. [这是我使用GitPages搭建的个人博客](https://meiko-zhang.github.io/).

由于 gitbook 书籍可以通过 `gitbook` 本地构建出 site 格式，所以可以直接将构建好的书籍直接放到 GitHub Pages 中托管，之后，可以通过如下地址访问书籍：

```
.github.io/
```

例如：这本书中使用的例子 ‘test’ 项目可以通过地址：*chengweiv5.github.io/test* 来访问。

当访问 *meiko-zhang.github.io/test* 时，会访问 *meiko-zhang/test* 项目的 *gh-pages* 分支的内容，所以需要为项目创建一个 *gh-pages* 分支，并且将静态站点内容放入其中。也就是说，test 项目将有如下两个分支：

- master, 保存书籍的源码
- gh-pages, 保存书籍编译后的 HTML 文件 

## 安装GitBook

我们需要通过Github上的源码生成对应的HTML才能在Github Pages上访问.所以需要安装Gitbook. 
请参考[GitBook的安装和使用方法.](https://blog.csdn.net/zijie_xiao/article/details/51110983)

我们的操作中还需要使用到Git. 
请参考[Git的安装和使用](https://www.linuxidc.com/Linux/2018-05/152610.htm) .

## 编译书籍

首先，创建一个文件夹,用户来存放要编译的书籍.

```bash
mkdir book && cd book1
```

将需要编译的数据源码仓库从Github上clone到当前文件夹中.

```bash
git clone git@github.com:meiko-zhang/test.git1
```

使用 `gitbook build` 将书籍内容输出到默认目录，也就是当前目录下的 *_book* 目录。

```
gitbook build1
```

build 失败的可能是配置出现了错误,可以根据提示修改book.json 的配置信息. 
注意:记得修改book.json后提交到github上. 

```bash
git add book.json
git commit -m "update book.json"
git push -u origin master123
```

## 创建 gh-pages 分支

执行如下命令来创建分支，并且删除不需要的文件：

```bash
git checkout --orphan gh-pages
git rm -f --cached -r .
git clean -df
rm -rf *~1234
```

现在，目录下应该只剩下 *_book* 目录了，首先，忽略一些文件：

```bash
echo "*~" > .gitignore
echo "_book" >> .gitignore
git add .gitignore
git commit -m "Ignore some files"1234
```

然后，加入 *_book* 下的内容到分支中：

```bash
cp -r _book/* .
git add .
git commit -m "Publish book"123
```

## 上传书籍内容到 GitHub

现在，可以将编译好的书籍内容上传到 GitHub 中 *book* 项目的 *gh-pages* 分支了，虽然这里还没有创建分支，上传和创建会一步完成！

```bash
git push -u origin gh-pages1
```

现在，书籍的内容已经上传到 GitHub 上，所以通过访问 *meik-zhanggithub.io/test* 就可以阅读 *test*这本书了！

参考此书籍,总结完善后发表本文,并将[此本书发布在Github Pages上](https://meiko-zhang.github.io/gitbook/).