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