

在Linux系统下使用 npm yum rpm  nginx Linux防火墙 firewall  去IOE运动



### 注册软链接， 和添加系统环境变量

```
yum / rpm
压缩文件安装tar.gz / tar.xz
	-src  源代码构建安装  make && make install
	- bin 注册环境变量安装   重启服务器会失效
	解压后有 bin 文件有二进制程序可以跑
	但是韩式要注册path环境变量将 bin 复制到path环境变量中 echo $PATH(查看环境变量)
	 root: export PATH=$PATH:+bin的目录 增加
	 
	 永久有效
	 查看 ~ 所有文件  隐藏文件.bash_profile文件
	 vim .bash_profile
	 在 PATH 里添加 bin文件目录
hell.sh shell 脚本格式文件 实现自动化操作
	写入命令 更改为可执行格式 +x 然后执行
```

**安装软件**

#### yum

- `yum` `yellowdog upgrade modified`   包管理工具
  - 前身  rpm  `redhat package manager`  
  - yum 服务器仓库
- `yum list insatlled | grep Nginx`
- `yum search Nginx` 找网络有没有资源
- `yum install nginx`
- yum update 更新
- 反向代理

#### rpm

- **红帽子包管理工具**
- `rpm -i `安装  +`vh`
- `rpm -e` 移除  +` vh`
- `rpm -vh` 查看安装过程
- `rpm -qa `查询所有 搜索软件包
- `-rpm -e` 移除
- `xargs` 参数化
  - `rpm -qa | grep jdk | xargs rmp -e` 将`grep` 搜索到的结果 `xargs`(作为参数用来) 进行后面的操作

 **服务开机自启**

- `systemctl enable mariadb`
- 停止开机自启
  - ` systemctl disable mariadb`  或者 `删除符号链接`



##### Linux系统防火墙firewall

- `systemctl start firewalld`/ `iptables`启动防火墙
- `firewall -cmd` 配置防火墙    开端口`--add-port=80/tcp  -- permanent -zone=public`
- 企业级防火墙  两层防火墙 DMZ -`Demilitary zone`



#### `nginx` HTTP服务器

##### 1.介绍

- `Apache`     `LAMP` `Linux Apache MySQL PHP`  做网站黄金组合

  - **Apache HTTP Server**（简称[**Apache**](https://baike.baidu.com/item/Apache/6265)）是[Apache软件基金会](https://baike.baidu.com/item/Apache%E8%BD%AF%E4%BB%B6%E5%9F%BA%E9%87%91%E4%BC%9A)的一个开放源码的网页服务器，可以在大多数计算机操作系统中运行，由于其多平台和安全性被广泛使用，是最流行的Web服务器端软件之一。它快速、可靠并且可通过简单的`API`扩展，将`Perl/Python`等解释器编译到服务器中。

- `Nginx`      `LNMP`   `LInux Apache MySQLO Python` 现在最快组合

  - `Nginx `是一个很强大的高性能[Web](https://baike.baidu.com/item/Web/150564)和[反向代理](https://baike.baidu.com/item/%E5%8F%8D%E5%90%91%E4%BB%A3%E7%90%86)服务器，它具有很多非常优越的特性：

    在连接高并发的情况下，`Nginx`是[Apache](https://baike.baidu.com/item/Apache/6265)服务器不错的替代品：`Nginx`在美国是做虚拟主机生意的老板们经常选择的软件平台之一。能够支持高达 50,000 个并发连接数的响应，感谢`Nginx`为我们选择了 `epoll and kqueue`作为开发模型。

`DNS` 将域名翻译成`IP`地址

- 停机
  - `nginx -s stop `停止

##### 2. 阿里云防火墙处理

- 实例 -> 管理 -> 实例安全组 -> 内网入方向规则

  - 安全组列表  ->配置规则 -> 入方向 -> 容许 ->  HTTP(80)
    - 授权对象 0.0.0.0

- 替换页面

  - `/usr/share/nginx/html  ` 进入页面目录
  - `404 /50x`
  - `config`文件  server  连接端口  
    - `cd /etc/nginx/  `  下面的 `nginx.config` 配置文件
    - 页面路径  root ..

- 上传文件

  - 先安装`Xftp.exe`     点击上传到`/usr/share/nginx/html `

  苹果下:  打开终端      输入命令:  `sftp`  root@公网`IP`

  - 连接成功 `  sftp>  ls  `      列出目录   get + 文件 下载文件
  - put 上传    put + 文件名



#### 去`IOE`运动

2008 阿里巴巴   去`IOE`运动  多台小型机器 组装可以构成性能好的服务器

IBM  小型机

Oracle  数据库

- `GFS/TFS/Tair  ` 自制 的数据存储系统    

`EMC(HP) `存储设备

Java  Spring框架

集群技术 分摊请求  负载均衡

- `LVS + keepalice `将普通服务器改为负载均衡服务器