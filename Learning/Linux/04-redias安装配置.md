

非关系型数据库,redias安装,配置redias





 `redias` 数据库  非关系型数据库

#### 1.下载

- Linux 下载到的是源代码 yum install 

#### 2.构建安装

**2.1 下载**

- 进入解压后的目录 有目录`Makfile 目录`
- 构建  `make`
- 安装 `make install`     (make && make install)

**2.2 改配置文件   `redias.config`**

- 进入`redias`文件夹
- 复制  `redias.config`
- 更改 61行  bind:
  - 原来绑定的是本机 
  - 改为的阿里云内网地址(末行模式 `!ifconfig `查看` inet `)172.2(内网地址)
- 84行 为端口 默认设置是 6379, 可自己更改
- `:/require  (搜索)  ` 480行 去注释  改pass 密码: `*****`

** 2.3 启动**

- `redis-server myredis.config > myredis.log &`

**2.4 连接**

- `redis-cli -h 172.24.(内网地址) -p 6379(端口号)`
- 进入后输入`auth + 密码 `
- 退出
  - 将后台移到前台   `ctrl + c `保存数据退出
  - `fg %1 `移动到前台
  - `bg %1` 移动到后台启动



