

### windows `git`操作

- ### 主要流程

```shell
windows本地操作
git init   (第一次要设置user.name, user.email)
git add .   
git status
git commit -m "修改说明"
git remote add origin <远端仓库url>
git push origin master   同步到远端
git pull 从远端下载版本到本地

远端克隆到本地
git clone <远端仓库的url>
cd 仓库名  进入克隆下的仓库
git branch <分支名>  创建自己的分支修改文件
git branch -d <分支名>  删除分支
git checkout <分支名>   切换分支
修改本地文件 
git add .
git status  (查看一下状态)
git commit -m "修改描述"
git remote add origin <远端的url>
git push origin master   <上传>   
	一般是以分支提交的,以master提交会覆盖主分支
git pull   <下载远端同步>

合并分支,   合并前先切换到 master 里
git merge <分支名>   默认的会将分支合并道 master里

日志
git log --online 
	用 --graph 选项，查看历史中什么时候出现了分支、合并
```





- `git init `将文件夹变文本地仓库
- `notepad test.txt `创建文件
- `git add + filename/git add . `将当前文件目录的所有文件添加版本控制
- 初次要设置用户名和邮箱
  - `git config --global user.name 'zhang'`
  - `git config --global user.email 'zhang.email'`

#### 本地仓库

- 初始化`git`
  - `git init `将文件夹变为本地仓库
- `git add + filename/git add . `将当前文件目录的所有文件添加版本控制
- `git commit -m "修改的内容,提交的原因"` 提交本地的修改后的版本
- `git status` 查看暂存区状态, 有没有修改文件 -commit 提交
- `git log` 查看日志

#### 版本控制

- `git reset --hard  版本号的前六位` 回到版本
- `git reflog` 查看历史日志
- `log --pretty=oneline`  单行查看日志

#### 撤回暂存区内容,  撤回错误的操作

- `git checkout--文件名`(不加表示撤掉所有)
  - 修改之后重新提交
- `Gitlab` 可以自己搭建服务器

### 添加到远端仓库, 具体操作

- **本地建仓库提交到远端**
  - `makdir hello`
  - `cd hello`
  - `git init`
  - `git add .`
  - `git status`
  - `git commit -m  ""说明"`
  - `git log `
- 链接远程仓库
  - `git remote add origin url`
- 提交
  - `git push -u origin master` **第一次**使用加  `-u`

### 从远程仓库克隆

- `git clone  url `在桌面克隆一个远程仓库
- `add . `
- `commit -m`
- `git push origin master `(origin 项目的别名) 将项目提交到主干上面
- `git pull `从远端同步到本地

#### 重置版本

- `git reset --hard   id`
- `git reflog`
- `git remote add origin (url)`
- `git push -u origin master`
- `git pull  (url)`

#### 远端已存在的项目,克隆到本地

- `git clone (url)`
- `cd hello `
- `git add .`
- `git checkout  --`
- `git commit -m  "说明"`
- `git push origin master`
- `git pull`



### git 使用流程

- `git cone <url>`
- `cd <dir>`
- `git branch  分支名 `   创建
- `git checkout 分支名 `  切换
- 合并   切换到`master `分支
  - `git merge  cool-function `将分支合并到master
  - `git push origin master`

