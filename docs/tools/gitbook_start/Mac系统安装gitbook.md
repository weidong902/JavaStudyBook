## Mac系统安装gitbook

###### 

# [Gitbook在 Mac 环境上的安装及使用](https://www.cnblogs.com/yuanpeng-java/p/10384300.html)

## 一.在 Mac 环境上搭建 gitbook

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
#1.安装node.js，在node.js官网下载，直接安装稳定版本。
https://nodejs.org/en/

#2.检测 node.js 是否安装成功
npm -v

#3.安装 gitboot 和命令行工具 -g 代表全局安装
sudo npm install gitbook -g 
sudo npm install -g gitbook-cli

#4.检测是否安装成功 v 大写
gitbook -V
gitbook -version

#更新 gitbook 命令行工具
sudo npm update gitbook-cli -g

#卸载 GitBook 命令
sudo npm uninstall gitbook-cli -g

#查看安装位置
which gitbook

#5.安装 gitboot editor,方便编辑书籍
https://legacy.gitbook.com/editor/osx

#6.安装calibre,calibre是一款非常方便的开源电子书转换软件
https://calibre-ebook.com/download

#7.将安装的calibre放在应用程序中,执行
sudo ln -s /Applications/calibre.app/Contents/MacOS/ebook-convert /usr/local/bin
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

##  

##  二.gitbook的使用

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
# 1.创建 mygitbook 文件夹，作为第一本书,并切换到这个文件夹下面
mkdir mygitbook && cd mygitbook

#2.初始化 gitbook 工作目录，创建必要的文件
gitbook init
#README.md - 项目的介绍都写在这个文件里。
#SUMMARY.md - GitBook 的目录结构在这里配置。

#3.编辑目录结构

#4.目录建好以后在根目录下执行命令,只支持2级目录：
gitbook init

#编写 gitbook 内容,重新编译
gitbook build

#5.在根目录执行命令,启动服务：
gitbook serve

#6.访问,用浏览器打开 http://localhost:4000/ 或 http://127.0.0.1:4000/ 查看显示书籍的效果。结束预览 ctrl+c

#7.生成电子书,依赖于Calibre
gitbook mobi ./ ./MyFirstBook.mobi
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

##  

## 三.将写好的文章推送到 github 上

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
#注册 GitHub.com 账号,并新建一个项目。在“Setting（设置）”页面获取到“Git URL（Git 链接）”

#在本地安装 git

#在本地新建一个文件夹，并通过 Git 命令把刚才新建的远程项目抓取到本地
$ mkdir MyFirstBook-Git
$ cd MyFirstBook-Git
$ git init
$ git pull https://git.gitbook.com/kindlefere/myfirstbook.git

#然后把本地项目“MyFirstBook”中的所有内容拷贝到刚才新建的文件夹中，如上面的“MyFirstBook-Git”。然后使用 Git 命令把本地的项目上传到远程
$ git add -A
$ git commit -m "提交说明"
$ git remote add gitbook https://git.gitbook.com/kindlefere/myfirstbook.git
$ git push -u gitbook master

#修改内容后只需要输入以下 Git 命令即可
$ git add [修改的文件]
$ git commit -m "提交说明"
$ git push -u gitbook master
```