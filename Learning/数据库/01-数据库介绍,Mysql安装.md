

**1.为什么要使用数据库?**

- 数据持久化  - 将数据转移到持久存储介质 (断电也不会是数据丢失)
- 可以高效的存储和处理数据, 检索数据

**2.为什么使用关系型数据库**

- 理论基础:

  - 集合论和关系代数

- 用二维表组织数据

  - 每张表叫做实体
  - 行   - 记录
  - 列   -  字段

- 关系型数据库, 表与表之间存在某种关系

  ​	

- 关系型数据库都有自己的编程语言 

  - SQL - 结构化查询语言
  - SQL在每种数据库会略有差别, 

**3. 数据库  - 数据库系统 - 数据管理系统**

- 数据库  DB 数据的集散地 (仓库)
- 数据库系统 DBS    包括了 DB  DBA(数据库管理员)  DBMS
- (r)DBMS - (关系型)数据库管理系统   (管理数据库的软件)
  - **关系型数据库管理软件**
    - MySQL  - 小巧强大
    - Oricle - 强大  商业智能分析  昂贵
    - DB2 - 强大  安全  商业智能  (银行  电商 金融)  昂贵
    - 微软 - SQL Server 
    - SQLite  - 移动端嵌入式关系型数据库   

**4.数据库服务器和客户端工具**

- SQLyog  -   Toad for MySQL   -  Navicat for MySQL
- 安装包和破解工具[ navicat120_crack.rar](http://10.7.152.68/%e8%bd%af%e4%bb%b6/navicat120_crack.rar)   -  [ navicat120_mysql_cs_x64.exe](http://10.7.152.68/%e8%bd%af%e4%bb%b6/navicat120_mysql_cs_x64.exe)   
- 也可以安装 Nacicat premium  更加强大的 navicat





#### 安装

##### 1.Linux安装mariadb, 启动进入mariadb

- `yum install mariadb-server mariadb` 安装mariadb
- 启动服务 
  - `systemctl start mariadb`
- 进入mariadb , 超级管理员初次登录密码是空,前面设置时选择跳过密码设置就好
  - `mysql -u root -p`
- 停止
  - `systemctl stop mariadb`
- 开机自启设置
  - `systemctl enable mariadb`
- 取消开机自启动
  - `systemctl disabl mariadb` 或者 `删除符号链接`
  - `centos6用 ``checkconfig` `serveice`配置开机自启

https://git.coding.net/zhangminglu/images.git

#### 2.windows安装mysql:

- 下载地址[ mysql-installer-community-5.7.21.0.msi](http://10.7.152.68/%e8%bd%af%e4%bb%b6/mysql-installer-community-5.7.21.0.msi)  
- 安装指南  
  - 选择`Custom `最后一个自定义安装
  - 选择`MySQLServer X64  `在旁边框中选择 `Mysql Client Programs`
  - 只安装 server 第一个 
  - 如果出现缺失的提示, 选择 该软件 Execute
  - ready to config  配置
    - Type and Networking   -> cofing Type Development Machine 
    - ​
- 出现缺少环境更新 .NET环境   微软官方下载    .NET4.5.2下载



启动和停止服务

- 打开widows服务 查找框  输入:  service.msc  找打 mysql 进行启动和停止
- cmd 输入  `net stop / start mysql` 启动或停止
- 选择 client utf-8 打开客户端
- `mysql -u root -p` 也可以链接数据库



### 使用MySQL

- `show databases;`  查看基础数据库     命令以 分号表示结束
- `select version(); `查看版本
- `?   ` 查看帮助



 我用**Navicat for MySQL**连接 MySQL

- locahost / 或者 回环地址 : 127.0.0.1  连接自己的服务器



进入后就可以使用了