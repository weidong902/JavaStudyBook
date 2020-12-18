

# 如何修改别人的github项目，并上传到他的github

[参考](https://blog.csdn.net/qq_41401062/article/details/89304072)

**实现这个过程主要操作过程为：**
先到别人的github上点击”fork“到自己的github，然后在从自己的github克隆项目，修改后上传到自己的github，之后点Create pull request就传到别人的github上。


**具体过程：**
**1.在所要修改的github上点击 “fork” 传入到自己的github（首先要有自己的igthub账号）**
![在这里插入图片描述](D:\gitStudy\test\JavaStudyBook\docs\tools\git使用\Untitled.assets\20190414225612399.png)
**2.之后打开git，首先建立仓库，并从自己的github地址克隆项目到本地**
（1）输入git init创建本地仓库（结果是在当前目录会出现.git文件）
（2）输入 git clone 你的github项目地址 （这一步是克隆项目到本地）
（3）就是你对项目进行一些代码上的修改等等（忽视这一步）


**3.将修改之后的项目上传到自己github里**
（1）输入 git add . （这一步是将修改的内容进行同步，须注意的是后面的 . 要有空格）在这个输入完后可以在输入git status查看进度
（2）输入git commit -m “注释内容” （这一步可以跳过，主要作用是对修改的地方进行注释，方便别人进行整个）
（5）输入 git push即可上传成功


**4.点击github上面的 "Pull request"**
![在这里插入图片描述](D:\gitStudy\test\JavaStudyBook\docs\tools\git使用\Untitled.assets\20190414231204911.png)


**5.之后点击 "Create pull request"即可完成**
![在这里插入图片描述](D:\gitStudy\test\JavaStudyBook\docs\tools\git使用\Untitled.assets\20190414231511225.png)

附录：
**1、最开始创建本地仓库并导入指定位置**
![在这里插入图片描述](D:\gitStudy\test\JavaStudyBook\docs\tools\git使用\Untitled.assets\20200225201641482.png)
**2、查看git登录的用户名和邮箱号**

```
git config user.name
git config user.email
12
```

**3、关于分支的说明**
当从Github克隆下来的项目，尽量不要在master主分支上进行修改，可创建一个本地分支（当创建分支后你会发现在分支上修改的内容，切换到master主分支上内容并未发生修改），分支修改完毕后在整合到主分支master上。
方法：创建分支时，必须要在git init的项目上才能创建

```
git branch									查看当前分支
git branch ****								创建分支
git checkout ****							切换分支
git checkout -b ****						创建和切换分支（整合在一起）
git merge ****								合并分支到当前分支上
git branch -d ****							删除某分支
123456
```

**4、团队协作开发说明**
首先需要在github上创建分支。一个伙伴先clone下来项目，在本地创建自己的分支，修改完之后就上传到远程分支里。
**（1）**
**若想将远程分支的内容合并到远程主分支上，则需要以下操作：**
1.先在本地分支中利用git pull将远程分支最新更新的内容克隆下来

```
git pull
1
```

若出现错误，可能是本地分支并未与远程分支建立连接，解决方法

```
git branch --set-upstream-to=origin/远程分支名 本地分支名
1
```

2.pull之后切换到本地的master分支里，在合并两个分支

```
gir checkout master
git merge dev
12
```

3.再push到远程master即可

```
git add .
git push origin master
12
```

如果你感觉合并后的内容有问题，你可以通过撤销合并恢复到以前状态。

```
git reset --hard HEAD
1
```

代码已经提交，撤销的方法是

```
git reset --hard ORIG_HEAD
1
```

**（2）冲突问题**
若多个伙伴同时修改同一个文件，并上传到分支会出现失败问题。只能同时一个伙伴进行合并。解决方法就利用
git pull更新最新的内容，在本地解决冲突问题再上传。
如何解决冲突：自己手动打开代码进行修改内容（新手学习，请勿喷）