# Git 基础

# 一、简介

- 什么是 Git？
  Git 是一个开源的分布式版本控制系统。最开始，Linus Torvalds 为了帮助管理 Linux 内核开发而开发。
- Git 的作用？
  版本控制：管理不同版本的文件，可以记录每次文件的改动。

- 分布式控制系统的优点：
  - 不需要中央服务器，安全性高。
  - 不需要联网，每个人本地都有一个库。



# 二、Git 安装

Git [下载地址](https://git-scm.com/downloads)，下载对应系统的安装包，一直 next 即可。

安装完成后，在开始菜单找到 Git -> Git Bash（或直接在桌面或文件夹中右键），这就是 Git 的命令窗口，可以进行 Git 操作。



# 三、Git 配置

Git 提供了一个叫做 git config 的工具，专门用来配置或读取相应的工作环境变量。

环境变量存放的位置：

- 系统中所有用户都适用：/etc/gitconfig，使用 git config --system 读写的是这个文件。
  位于安装目录
- 适用于当前用户：~/.gitconfig，使用 git config --global 读写的是这个文件。
  $HOME 变量指定的目录，一般是 C 盘 User 目录下
- 适用于当前项目：.git/config

上边三个配置，下层会覆盖上层的相同配置，即 .git/config 里的配置会覆盖 /etc/gitconfig 中的同名变量。

### 用户信息

配置个人的用户名和电子邮件地址：

```bash
$ git config --global user.name "你的用户名"
$ git config --global user.email "你的邮箱"
```

以上使用了 --global 选项，更改的配置文件就是位于用户主目录下的那个，以后所有项目都会默认使用这里配置的信息。如果在某个特定项目中使用其它 username 和 email，去掉 --global，在项目中重新配置即可。

### 查看配置信息

```bash
$ git config --list
```

有时候会看到重复的变量名，那就说明它们来自不同的配置文件（比如 /etc/gitconfig 和 ~/.gitconfig），不过最终 Git 实际采用的是最后一个。



# 四、Git 概念

- 工作区
  电脑里能看到的目录。
- 暂存区（索引）
  位于版本库中，一般放在 .git/index 中。
- 版本库
  工作区中的隐藏目录 .git ，就是 Git 的版本库。
- 分支
  分支是版本控制系统中的重要特性。
  一个项目有多个分支，主线就是主分支，支线就是其它分支。其他分支最终合并到主分支上。
  主分支一般是 master
- HEAD
  我们可以把 HEAD 理解成 “指针”，指向当前分支

<img src="https://cdn.jsdelivr.net/gh/ShameYang/images/img/Git基础1.png" style="zoom: 80%;" />



# 五、Git 基本操作

## 初始化仓库（init）

使用 `git init` 命令初始化仓库：

```bash
$ git init # 使用当前文件夹作为 git 仓库
$ git init <directory> # 指定目录作为 git 仓库
```

执行完后会生成一个 .git 目录。

## 克隆仓库（clone）

使用 `git clone` 拷贝一份远程仓库，也就是下载一个项目。

```bash
$ git clone <repo> # 拷贝到当前文件夹
$ git clone <repo> <directory> # 新建一个文件夹，拷贝到新建文件夹中
```

## 暂存（add）

使用 `git add` 命令，将工作区中的文件提交到暂存区：

```bash
$ git add <file> # 添加单文件
$ git add . # 当前工作区中的所有新文件和对已有文件的改动提交至暂存区，但不包括被删除的文件
$ git add -u # 将当前整个工作区中被修改和被删除的文件提交至暂存区
$ git add -A # git add -all 的简写，将工作区中所有改动提交至暂存区
```

## 提交（commit）

使用 `git commit` 命令，将暂存区提交到本地仓库：

```bash
$ git commit -a # 修改文件后不需要执行 git add 命令，直接来提交
$ git commit -m <message> # 提交并备注信息
```

## 推送（push）

使用 `git push` 命令，将本地分支版本推送到远程仓库合并：

```bash
$ git push <远程主机名> <本地分支名>:<远程分支名>
$ git push <远程主机名> <本地分支名> # 本地分支名与远程分支名相同，则可以省略冒号
```

## 拉取（fetch）

`git fetch` 命令用于从远程获取代码库。

该命令执行完后需要执行 `git merge` 合并远程分支到你所在的分支。

## 拉取合并（pull）

`git pull` 命令用于从远程获取代码并合并本地的版本。

`git pull` 就是 `git fetch` 和 `git merge FETCH_HEAD ` 的缩写。

```bash
$ git pull <远程主机名> <远程分支名>:<本地分支名>
```

## 查看仓库状态

`git status` 命令可以查看在你上次提交之后是否有对文件进行再次修改。

```bash
$ git status
$ git status -s # 获得简短的输出结果
```

## 查看历史提交记录

```bash
$ git log [选项] [分支名/提交哈希]
```



# 六、分支操作

分支是 Git 分布式版本控制系统中的重要特性。使用分支，可以在不影响主分支的情况下进行工作。

Git 分支实际上是指向更改快照的指针。

## 创建分支

```bash
$ git branch <branchname>
```

## 切换分支

切换分支时，Git 会用该分支的最后提交的快照替换你的工作目录的内容。

```bash
$ git checkout <branchname>
$ git checkout -b <branchname> # 创建新分支，并立即切换到该分支下
```

## 查看分支

```bash
$ git branch
```

## 删除分支

```bash
$ git branch -d <branchname>
```

## 合并分支

```bash
$ git merge <from-branchname>
```



# 七、远程仓库 GitHub

> 详情参考：[菜鸟文档](https://www.runoob.com/git/git-remote-repo.html)

简介部分介绍过，Git 没有中央服务器。如果想要分享代码或与他人合作开发，需要将数据放到一台服务器上，也就是所谓的远程仓库中。

这里我们以 GitHub 为例。

## 添加远程仓库（remote add）

```bash
$ git remote add <remote_name> <remote_url>
```

这里 remote_name 一般为 origin，remote_url 为 GitHub 仓库中的 URL

## 连接 GitHub

本地 Git 仓库和 GitHub 仓库之间的传输是通过SSH加密的，所以我们需要配置验证信息：

使用以下命令生成 SSH Key：

```bash
$ ssh-keygen -t rsa -C "Github 上注册的邮箱"
```

之后会要求确认路径和输入密码，我们这使用默认的一路回车就行。

成功的话会在 ~/ 下生成 .ssh 文件夹，进去，打开 id_rsa.pub，复制里面的 key，然后在 GitHub Settings 中 Add SSH key。



验证连接：

```bash
$ ssh -T git@github.com
```

之后输入 yes

## GitHub 新建仓库

这里就不多说了，不会的朋友可以参考文档或教程

## 查看当前的远程库

```bash
$ git remote # 列出当前仓库中已配置的远程仓库。
$ git remote -v # 列出当前仓库中已配置的远程仓库，并显示它们的 URL。
$ git remote show <remote_name> # 显示指定远程仓库的详细信息，包括 URL 和跟踪分支。
```

## 推送到远程仓库

add、commit 执行之后，使用 push 推送到远程仓库（基本操作中介绍过）

```bash
$ git push <远程主机名> <本地分支名>:<远程分支名>
$ git push <远程主机名> <本地分支名> # 本地分支名与远程分支名相同，则可以省略冒号
```

## 删除远程仓库（remote remove）

```bash
$ git remote remove <remote_name> # 从当前仓库中删除指定的远程仓库。
```
