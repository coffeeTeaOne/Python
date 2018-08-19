---
title: 01-用 hexo 部署自己的博客到coding 和 github 小结
date: 2018-01-04 11:00:51
tags: hexo博客建设
---

<h4 style="color: #228B22;">如何将 Hexo  生成博客同步到-github  coding     绑定自定义域名</h4>



- 安装 [node.js](https://nodejs.org/en/)
- 安装 [git](https://git-scm.com/downloads)

- 首先需要在自己的 coding 和 github 上生成自己邮箱的公钥,并且要在生成公约时勾选 容许推送的权限

  - 生成公钥 在 cmd 输入:`ssh-keygen -t rsa -C <your_email@example.com>`( 你的邮箱) 
  - 生成后公钥在 users/administrator/.ssh/id_rsa.pub  记事本打开,复制后粘贴到自己的 coding  和 github 对应的位置

- 在桌面兴建文件夹  我建的文件夹是 hexo

  - git init

  - 初始化文件夹   `hexo init`  

    在`hexo / _config.yuml`最后要加代码

```shell
type: git
  repo:   #coding 仓库ssh 
  repo:   #github 仓库ssh 
  branch: master
```

- 关联git
  - `npm install --save hexo-deployer-git`
- 在 sourse/ _post/ 文件夹中可以用命令
  - `hexo new 文件名 `  新建自己的博客文章
- hexo g  创建静态博客文件
- hexo s   打开hexo 提供的服务器  在 localhost: 4000 查看博客预览效果
- hexo d  推送到自己的 coding  和 github 仓库
- 在仓库中选择pages 服务  就可以生成博客页面了  coding 的静态页面可能会有部分样式加载不出来的情况 建议使用 动态pages 



- 介绍一下hexo下用到的命令：

```
hexo init myblog

$ hexo g/generate #生成静态文件
$ hexo s/server #启动服务器，主要用来本地预览
$ hexo d/deploy #将本地文件发布到github或Coding上
$ hexo n/new "postName"#新建一篇文章
$ hexo n/new page "pageName" #新建页面
$ hexo h/help # 查看帮助
$ hexo v/version #查看Hexo的版本
```

###  github

- 同步github仓库
  - 同样需要生成公钥, 然后在配置文件中添加` repo: github--repo-ssh`
  - `npm install --save hexo-deployer-git`
  - `hexo clean && hexo g && hexo d`
- 绑定自定义域名 
  - 在  soures 文件目录下  vim CHAME 写入自己的自定义域名
  - 在githunb 中的  setting 中 找到 pages 服务  写入自己的域名
  - 阿里云解析自己的域名  添加 CHAME 解析



- 解除 github  对自定义域名的绑定
  - `hexo clean  `   清楚绑定的文件
  - 然后,一定要清楚浏览器的缓存, 浏览器缓存自定义的跳转,不清除缓存还是会继续跳转的

 

 