---
title: 12-Git学习笔记02
date: 2018-04-13 12:10:28
tags: Git
---

<h4 style="color: #228B22;">git分支 合并 暂时储藏分支  标签  链接多个仓库</h4>



#### 1.git push -u origin master

- 由于远程仓库是空的,第一次推送master分支时, 加上 -u , git 不但会把本地的master分支内容推送到远程的新的master分支,还会把本地的master分支和远程的master分支关联起来, 在以后推送或者拉取时就可以简化命令

```shell
git checkout -b zhang   # 创建分支并切换到分支
	git branch zhang
	git checkout zhang
git branch -d zhang   # 删除分支 zhang
git merge zhang # 合并分支
```



#### 2.分支合并冲突

```shell
git log --graph  #查看分支合并图
```



#### 3.撤销缓存区的内容

`git rm --cached <file>`



#### 4.暂存分支任务

```shell
git branch # 当前在zhang分支工作
git stash   # 将当前分支暂时'储藏起来', 等完成别的分支任务之后对其进行恢复
git checkout master  # 切换到master分支
git checkout -b bug-101  # 创建新的修复bug的分支
完成修复任务后
git add .
git commit -m "bug修复"
git checkout master
git merge --no-ff -m "bug修复" bug-101
git checkout zhang
git status   # 查看工作区状态还是干净的
git stash list #查看暂时储藏起来的分支
git apply stash@{0}  #恢复前面储藏的内容
git stash drop
也可以使用一个命令
	git stash pop # 恢复的同时也删除了原先储藏的内容
```



#### 5.强行删除分支

`git branch -D <name>` 强行删除分支



#### 6.推送分支

```shell
git push origin <name>  # 推送本地分支
```



#### 7.标签

```shell
切换到分支,然后打标签
git checkout zhang
git tag v1.0
git tag  #查看标签
```

- 命令`git tag <name>`用于新建一个标签，默认为HEAD，也可以指定一个`commit id`；
- 创建带有说明的标签，用`-a`指定标签名，`-m`指定说明文字
- `git tag -a <tagname> -m "blablabla..."`可以指定标签信息；
- `git tag -s <tagname> -m "blablabla..."`可以用PGP签名标签；
- 命令`git tag`可以查看所有标签。
- 命令`git push origin <tagname>`可以推送一个本地标签；
- 命令`git push origin --tags`可以推送全部未推送过的本地标签；
- 命令`git tag -d <tagname>`可以删除一个本地标签；
- 命令`git push origin :refs/tags/<tagname>`可以删除一个远程标签。

```shell
如果标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除：
$ git tag -d v0.9
Deleted tag 'v0.9' (was 6224937)
然后，从远程删除。删除命令也是push，但是格式如下：
$ git push origin :refs/tags/v0.9
To git@github.com:michaelliao/learngit.git
 - [deleted]         v0.9
```



#### 8.查看远程库的信息.

```shell
git remote -v # 显示详细信息
```

- 删除本地已经连接的远程库的 链接
  - `git remote rm origin`
- 然后再次关联另外一个远程库
  - `git remote add origin <链接>`



#### 9.关联多个远程库

- 我们先删除已关联的名为`origin`的远程库：

```shell
git remote rm origin
```

- 然后，先关联GitHub的远程库：

```shell
git remote add github git@github.com:michaelliao/learngit.git
```

- 注意，远程库的名称叫`github`，不叫`origin`了。
- 接着，再关联码云的远程库：

```shell
git remote add gitee git@gitee.com:liaoxuefeng/learngit.git
```

- 同样注意，远程库的名称叫`gitee`，不叫`origin`。
- 现在，我们用`git remote -v`查看远程库信息，可以看到两个远程库：

```shell
git remote -v
gitee    git@gitee.com:liaoxuefeng/learngit.git (fetch)
gitee    git@gitee.com:liaoxuefeng/learngit.git (push)
github    git@github.com:michaelliao/learngit.git (fetch)
github    git@github.com:michaelliao/learngit.git (push)
```

- 如果要推送到GitHub，使用命令：

```shell
git push github master
```

- 如果要推送到码云，使用命令：

```shell
git push gitee master
```

这样一来，我们的本地库就可以同时与多个远程库互相同步



#### [使用Git忽略某些特殊的文件](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013758404317281e54b6f5375640abbb11e67be4cd49e0000)

#### [给Git指令设置别名](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001375234012342f90be1fc4d81446c967bbdc19e7c03d3000)

- 设置简短 的别名
  - `git config --global alias.st status`

#### [搭建Git服务器](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/00137583770360579bc4b458f044ce7afed3df579123eca000)





本文主要是通过廖雪峰的博客学习记的笔记,学习更多上请[廖雪峰Git教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)的博客, 也感谢博主的文章让我受益匪浅!!