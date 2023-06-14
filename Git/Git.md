## Git 常用命令

提交代码到 GitHub

```
$ git init - 初始化仓库（初始化一次即可）

$ git add - 将文件添加到暂存区
$ git status - 查看仓库状态
$ git commit -m "..." - 提交and提交信息到本地仓库

$ git remote add [alias] [url] - 创建远程仓库 （创建一次即可）
$ git push -u [alias] [branch] - 提交到远程仓库
```



## remote

```
$ git remote - 远程仓库名
$ git remote -v - 显示所有仓库
$ git remote show [url] - 仓库信息
```



## git错误

- error: failed to push some refs to https://github.com/

  远程库与本地库不一致

  ```
  $ git pull --rebase origin master
    把远程库中的更新合并到（pull=fetch+merge）本地库中，--rebase的作用是取消掉本地库中刚刚的commit，并把他们接到更新后的版本库之中
  
  $ git push origin master
  ```

- .gitignore配置不生效

  解决方法: git清除本地缓存（改变成未track状态），然后再提交

  ```
  $ git rm -r --cached .
  $ git add .
  $ git commit -m 'update .gitignore'
  $ git push -u origin master
  ```

- kex_exchange_identification

  ```
  $ ssh -T git@github.com 验证
  $ ssh -T git@github.com -p 22 指定端口号为 22
  ```

  

## commit 注释

add \<file> 然后 commit 即可，不使用 add .
