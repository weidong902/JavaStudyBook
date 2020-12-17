## Git使用

* Git安装

* GitHub账户注册

* 上传

  * 新建文件夹作为本地版本库：

    ```shell
    mkdir testPushGitHub
    cd testPushGitHub
    # 单行注释(shell脚本)
    # 将本文件夹变成Git可管理的仓库 git init 
    git init 
    
    git add . #将文件夹目录下所有提交到缓存区，也可（git add xx.txt）
    git status # 查看现有状态
    git commit -m "写些注释，不写注释提交不了" #将文件提交到本地仓库
    
    #连接远程仓库：Git仓库与GitHub仓库之间传输是通过ssh加密的，所以初次需要设置下。
    # mac下 
    cd ~/.ssh #(如果没有创建 mkdir ~/.ssh)
    
    ssh-keygen -t rsa -C "youremail@example.com"
    #一路回车
    cat ~/.ssh/id_rsa.pub
    # github -->settings-->SSH and GPG keys-->NEW SSH key 将本地电脑中id_rsa.pub复制到
    # 创建远程仓库
    # 如果之前电脑
    git branch -M main
    git remote add origin git@github.com:weidong902/testPushGitHub.git
    git push -u origin main
    ```
  ```

  
  
  ```

<img src="%E5%B0%86%E6%9C%AC%E5%9C%B0%E9%A1%B9%E7%9B%AE%E5%90%8C%E6%AD%A5%E5%88%B0GitHub%E4%B8%8A.assets/image-20201215211229044.png" alt="image-20201215211229044" style="zoom:50%;" />

  

<img src="%E5%B0%86%E6%9C%AC%E5%9C%B0%E9%A1%B9%E7%9B%AE%E5%90%8C%E6%AD%A5%E5%88%B0GitHub%E4%B8%8A.assets/image-20201215212254880.png" style="zoom:50%;" />

<img src="%E5%B0%86%E6%9C%AC%E5%9C%B0%E9%A1%B9%E7%9B%AE%E5%90%8C%E6%AD%A5%E5%88%B0GitHub%E4%B8%8A.assets/image-20201215212319191.png" style="zoom:80%;" />

* **创建仓库**
* <img src="%E5%B0%86%E6%9C%AC%E5%9C%B0%E9%A1%B9%E7%9B%AE%E5%90%8C%E6%AD%A5%E5%88%B0GitHub%E4%B8%8A.assets/image-20201215212947579.png" />

![](%E5%B0%86%E6%9C%AC%E5%9C%B0%E9%A1%B9%E7%9B%AE%E5%90%8C%E6%AD%A5%E5%88%B0GitHub%E4%B8%8A.assets/image-20201215213211612.png)

```
检查有没有设置过
git config user.name
git config user.email

设置用户名和邮箱
git config --global user.name "你的用户名"
git config --global user.email "你的邮箱"

修改你的用户名和邮箱
git config --global --replace-all user.name "你的用户名"
git config --global --replace-all user.email "你的邮箱"

生成密钥
ssh-keygen -t rsa -C '上面的邮箱'
代码参数含义：

-t 指定密钥类型，默认是 rsa ，可以省略。
-C 设置注释文字，比如邮箱。
-f 指定密钥文件存储文件名。


```

