



部署flask：

1. 更新ubuntu的源
   sudo apt-get update

2. 安装mysql
   sudo apt install mysql-server mysql-client

3. 修改mysql配置

   cd /etc/mysql/mysql.conf.d
   修改mysqld.conf 讲bind_address注释

4. 修改配置

   use mysql；

   GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '123456' WITH GRANT OPTION;

   flush privileges; 

5. 重启mysql
   service mysql restart

1. 安装Nginx：

   sudo apt-get install nginx

2. 安装pip3

   apt install python3-pip

1. 安装uWSGI以及uWSGI对于Python的支持：

   pip3 install uwsgi

2. 修改总的nginx的配置的文件

   vim  /etc/nginx/nginx.conf

3. 配置nginx的文件



```shell
server {

    listen       80;
    server_name 47.92.73.20 localhost;

    access_log /home/app/logs/access.log;
    error_log /home/app/logs/error.log;

    location / {
        include uwsgi_params;
        uwsgi_pass 127.0.0.1:8890;
        uwsgi_param UWSGI_CHDIR /home/app/src/s_aj;
    	uwsgi_param UWSGI_SCRIPT manage:app;   # 启动flask的文件:Flask的实例

	}
}
```

​	

1. 配置uwsgi的文件

```python
[uwsgi]

socket=127.0.0.1:8890

chdir=/home/src/day06

pythonpath=/usr/local/python3/bin/python3

pythonhome=/home/env/sjenv

callable=app;  # 回调的flask实例

logto = /home/logs/uwsgi.log

```





后台运行：

```
nohup python3 manage.py runserver -p 80 -h 0.0.0.0 -d &
```
