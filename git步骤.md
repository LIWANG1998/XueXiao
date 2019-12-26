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

# 六、分支管理

## 1. 分支分类

1. master：主分支
2. dev：开发时所用的分支
3. bug：解决bug的分支
4. feature：开发新功能分支

## 2. 分支管理策略

在实际开发中，我们应该按照几个基本原则进行分支管理：

首先，`master` 分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

那在哪干活呢？干活都在 `dev` 分支上，也就是说，`dev` 分支是不稳定的，到某个时候，比如1.0版本发布时，再把 `dev` 分支合并到 `master` 上，在 `master` 分支发布1.0版本；

你和你的小伙伴们每个人都在 `dev` 分支上干活，每个人都有自己的分支，时不时地往 `dev` 分支上合并就可以了。

所以，团队合作的分支看起来就像这样：

[![19](https://user-images.githubusercontent.com/12387544/35033548-13f2c9c2-fba6-11e7-9d61-9fffd6296fb4.png)](https://user-images.githubusercontent.com/12387544/35033548-13f2c9c2-fba6-11e7-9d61-9fffd6296fb4.png)

## 3. 分支图解

每次提交，Git都把它们串成一条时间线，这条时间线就是一个分支。截止到目前，只有一条时间线，在Git里，这个分支叫主分支，即 `master` 分支。`HEAD` 指向的就是当前分支。

一开始的时候，`master` 分支是一条线，Git用 `master`指向最新的提交，再用`HEAD`指向`master`，就能确定当前分支，以及当前分支的提交点：

[![10](https://user-images.githubusercontent.com/12387544/35029490-18c5bb4a-fb96-11e7-8238-8f4f052df91d.png)](https://user-images.githubusercontent.com/12387544/35029490-18c5bb4a-fb96-11e7-8238-8f4f052df91d.png)

每次提交，`master` 分支都会向前移动一步，这样，随着你不断提交，`master`分支的线也越来越长。

当我们创建新的分支，例如 `dev` 时，Git 新建了一个指针叫 `dev`，指向 `master` 相同的提交，再把 `HEAD` 指向 `dev`，就表示当前分支在 `dev` 上：

[![11](https://user-images.githubusercontent.com/12387544/35029472-04b8171a-fb96-11e7-9545-32640704af87.png)](https://user-images.githubusercontent.com/12387544/35029472-04b8171a-fb96-11e7-9545-32640704af87.png)

从现在开始，对工作区的修改和提交就是针对 `dev` 分支了，比如新提交一次后，`dev` 指针往前移动一步，而 `master` 指针不变：

[![12](https://user-images.githubusercontent.com/12387544/35029375-a83c1f7c-fb95-11e7-916d-858d0b248c7c.png)](https://user-images.githubusercontent.com/12387544/35029375-a83c1f7c-fb95-11e7-916d-858d0b248c7c.png)

假如我们在 `dev` 上的工作完成了，就可以把 `dev` 合并到 `master` 上。Git怎么合并呢？最简单的方法，就是直接把 `master` 指向 `dev` 的当前提交，就完成了合并.

[![13](https://user-images.githubusercontent.com/12387544/35029358-93a32074-fb95-11e7-878e-286f84ee5c82.png)](https://user-images.githubusercontent.com/12387544/35029358-93a32074-fb95-11e7-878e-286f84ee5c82.png)

合并完分支后，甚至可以删除`dev`分支。删除`dev`分支就是把`dev`指针给删掉，删掉后，我们就剩下了一条`master`分支：

[![14](https://user-images.githubusercontent.com/12387544/35029189-b79adefa-fb94-11e7-94f1-1e8958b269fa.png)](https://user-images.githubusercontent.com/12387544/35029189-b79adefa-fb94-11e7-94f1-1e8958b269fa.png)

## 4. 分支常用指令

- `git branch`：查看分支
- `git branch `：创建分支
- `git checkout `：切换分支
- `git checkout -b `：创建并切换分支
- `git branch -d `：删除分支
- `git branch -D `：强行删除分支（用在分支还未合并就要删除的情况）
- `git merge `：合并分支
- `git stash`：保存工作状态
- `git stash list`：查看保存的工作状态
- `git stash pop`：恢复工作状态
- `git stash apply`：恢复工作状态
- `git stash drop`：删除工作状态

## 5. 解决冲突

比如在 master 分支和 feature 分支上同时对某个文件进行了修改并且提交，如下所示：

[![16](https://user-images.githubusercontent.com/12387544/35031675-ea605374-fb9e-11e7-99c6-b79ed67c7165.png)](https://user-images.githubusercontent.com/12387544/35031675-ea605374-fb9e-11e7-99c6-b79ed67c7165.png)

那我们在合并分支的时候就会遇到冲突的情况，遇到冲突之后，直接到冲突的文件解决冲突，然后再次 add + commit即可，此时，合并之后的结构如下：

现在，`master` 分支和 `feature`分支变成了下图所示：

[![17](https://user-images.githubusercontent.com/12387544/35031759-4bef2796-fb9f-11e7-877e-3eb54a7cebca.png)](https://user-images.githubusercontent.com/12387544/35031759-4bef2796-fb9f-11e7-877e-3eb54a7cebca.png)

通过如下指令可以查看分支的合并情况：

```
$ git log --graph --abbrev-commit --pretty=oneline
```

合并分支时，可添加 `--no-ff` 参数保留分支信息：

```
$ git merge --no-ff -m "合并信息" <分支名>
```
