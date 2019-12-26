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
- ..