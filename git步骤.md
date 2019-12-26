查看git版本

```
$ which git
/usr/local/bin/git
$ git --version
git version 2.23.0
```

提示：windows 下需将 *which* 替换成 *where*.

## 1、配置用户信息

配置个人的用户名称和电子邮件地址：

```
$ git config --global user.name  "Your Name"
$ git config --global user.email "email@example.com"
```

## 2、查看配置信息

输入指令

```
$ git config --list
```

# 三、Git 工作流

你的本地仓库由 git 维护的三棵“树”组成。第一个是你的 工作目录，它持有实际文件；第二个是 缓存区（Index），它像个缓存区域，临时保存你的改动；最后是 HEAD，指向你最近一次提交后的结果。

[![img](https://github.com/LiHongyao/Blogs/raw/master/IMGS/git-trees.png)](https://github.com/LiHongyao/Blogs/blob/master/IMGS/git-trees.png)

# 四、Git 基本操作

- `git init`：创建版本库
- `git add .`：提交至缓存区
- `git commit -m ""`：提交至版本库
- `git status`：查看工作区状态
- `git diff`：查看修改
- `git restore <文件>`：撤销修改
- `git checkout . `：撤销修改
- `git restore --staged <文件>`：撤销暂存
- `git log --pretty=oneline --abbrev-commit` ：查看历史版本
- `git reset --hard `：版本回退
- `git reflog`：查看命令历史
- `git rm <文件>`：删除文件

# 五、远程仓库

https://github.com/

由于你的本地Git仓库和GitHub仓库之间的传输是通过SSH加密的，所以，需要一点设置：

**第1步：**创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有 ”id_rsa“ 和 ”id_rsa.pub“ 这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：

```
$ ssh-keygen -t rsa -C "youremail@example.com"
```

你需要把邮件地址换成你自己的邮件地址，然后一路回车，使用默认值即可，由于这个Key也不是用于军事目的，所以也无需设置密码。

如果一切顺利的话，可以在用户主目录里找到 ”.ssh“ 目录，里面有 ”id_rsa“ 和 ”id_rsa.pub” 两个文件，这两个就是SSH Key的秘钥对， ”id_rsa“ 是私钥，不能泄露出去， ”id_rsa.pub” 是公钥，可以放心地告诉任何人。

> 提示：
>
> 在windows系统下，如果出现 “ssh-keygen” 不是内部或外部命令，解决的办法是：
>
> 1. 首先找到 “ssh-keygen” 的安装目录，一般位于 “C:\Program Files\Git\usr\bin” 下。
> 2. 将该路径添加环境变量的Path字段中。

**第2步：**

登陆Github -> 点击右上角个人头像 -> 点击Settings进入设置页面 ->

点击SSH and GPG keys选项 -> 点击右上角New SSH key ->

在添加面板中，

Title：设置SSH key 标题，可任意填写

Key：将 *”id_rsa.pub”* 文件内容拷贝至此

然后点击 Add SSH Key 就可以看到你创建的SSH Key了，如下图所示：

[![img](https://github.com/LiHongyao/Blogs/raw/master/IMGS/git-sshkey.png)](https://github.com/LiHongyao/Blogs/blob/master/IMGS/git-sshkey.png)

为什么GitHub需要SSH Key呢？因为GitHub需要识别出你推送的提交确实是你推送的，而不是别人冒充的，而Git支持SSH协议，所以，GitHub只要知道了你的公钥，就可以确认只有你自己才能推送。

当然，GitHub允许你添加多个Key。假定你有若干电脑，你一会儿在公司提交，一会儿在家里提交，只要把每台电脑的Key都添加到GitHub，就可以在每台电脑上往GitHub推送了。

最后友情提示，在GitHub上免费托管的Git仓库，任何人都可以看到喔（但只有你自己才能改）。所以，不要把敏感信息放进去。

如果你不想让别人看到Git库，有两个办法，一个是交点保护费，让GitHub把公开的仓库变成私有的，这样别人就看不见了（不可读更不可写）。另一个办法是自己动手，搭一个Git服务器，因为是你自己的Git服务器，所以别人也是看不见的。这个方法我们后面会讲到的，相当简单，公司内部开发必备。

确保你拥有一个GitHub账号后，我们就即将开始远程仓库的学习。

## 1、添加远程库

首先，登陆GitHub，然后，在右上角找到点击 `+` 按钮，再选择 “New Repository” 创建一个新的仓库：

[![img](https://github.com/LiHongyao/Blogs/raw/master/IMGS/git-create-repository.jpg)](https://github.com/LiHongyao/Blogs/blob/master/IMGS/git-create-repository.jpg)

关联远程仓库：

[![img](https://github.com/LiHongyao/Blogs/raw/master/IMGS/git-remote.jpg)](https://github.com/LiHongyao/Blogs/blob/master/IMGS/git-remote.jpg)

```
# 关联远程仓库
$ git remote add origin https://github.com/LiHongyao/Teaching.git
# 推送远程仓库
$ git push -u origin master
```

推送成功后，可以立刻在GitHub页面中看到远程库的内容。从现在起，只要本地作了提交，就可以通过如下命令更新远程仓库：

```
$ git push origin master
```

**SSH 警告**

当你第一次使用Git的`clone`或者`push`命令连接GitHub时，会得到一个警告：

```
The authenticity of host 'github.com (xx.xx.xx.xx)' can't be established.
RSA key fingerprint is xx.xx.xx.xx.xx.
Are you sure you want to continue connecting (yes/no)?
```

这是因为Git使用SSH连接，而SSH连接在第一次验证GitHub服务器的Key时，需要你确认GitHub的Key的指纹信息是否真的来自GitHub的服务器，输入`yes`回车即可。

Git会输出一个警告，告诉你已经把GitHub的Key添加到本机的一个信任列表里了：

```
Warning: Permanently added 'github.com' (RSA) to the list of known hosts.
```

这个警告只会出现一次，后面的操作就不会有任何警告了。

如果你实在担心有人冒充GitHub服务器，输入`yes`前可以对照[GitHub的RSA Key的指纹信息](https://help.github.com/articles/what-are-github-s-ssh-key-fingerprints/)是否与SSH连接给出的一致。

## 2、克隆远程库

在终端通过如下指令即可克隆远程仓库：

```
$ git clone <仓库地址>
```

仓库地址有如下两种形式：

```
1. https://github.com/用户名/仓库名.git
2. git@github.com:用户名/仓库名.git
```

> 提示：用第2中地址形式比第1中速度更快。

当你从远程仓库克隆时，实际上Git自动把本地的`master`分支和远程的`master`分支对应起来了，并且，远程仓库的默认名称是`origin`。

要查看远程库的信息，用 `git remote`：

```
$ git remote
origin
```

或者，用 `git remote -v` 显示更详细的信息：

```
$ git remote -v
origin	git@github.com:LiHongyao/GITProj.git (fetch)
origin	git@github.com:LiHongyao/GITProj.git (push)
```

上面显示了可以抓取和推送的 `origin` 的地址。如果没有推送权限，就看不到push的地址。