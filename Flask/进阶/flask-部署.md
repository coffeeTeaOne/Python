flask-部署



centOS

前面的配置基本都和  Django  的配置相同， 只需要 更改 Nginx  和 uwsgi 文件



安装  mamriadb



netstat  -lnpt  查看进程



nohup 命令  后台运行



kill -9   进程号

nohup python manage.py runserver -h -p -d &  后台运行



```shell

ajnginx.config

server {
    listen    80;
    server_name 39.104.171.126 localhoust;  # 有域名 可以添加域名
    access_log /home/logs/access.log;
    error_log /home/logs/error.log;
    
    location / {
        include uwsgi_params;
        uwsgi_pass 127.0.0.1:8890;  # uwsgi 开的端口 必须一致
    
    	uwsgi_param UWSGI_CHDIR /home/src/aj;
    	uwsgi_param UWSGI_SCRIPT manage:app;
}



uwsgi.ini  文件配置

socket=127.0.0.1:8890;
chdir=/home/src/aj    #项目文件
pythonpath=/usr/local/python3.6/bin/python3  # python 路径
pyhtonhome=/home/env/aj_env   # 虚拟环境
callable=app
logto=/home/logs/uwsgi.log    # log存放目录


# 总 nginx 配置文件

include /home/congfig/ajnginx.config
```



```sgell
uwsgi --ini uwsgi.ini

tail -f uwsgi.log

重启  nginx 


配置  静态文件    base_dir

BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
```



```
 [uwsgi]
  2 projectname = axf
  3 base = /home/src
  4 
  5 # 守护进程
  6 master = true
  7 
  8 # 进程个数
  9 processes = 4
 10 
 11 # 虚拟环境
 12 pythonhome = /home/env/axfenv
 13 
 14 # 项目地址
 15 chdir = %(base)/%(projectname)
 16 
 17 # 指定python版本
 18 pythonpath = /usr/local/python3.6/bin/python3
 19 
 20 # 指定uwsgi文件
 21 module = %(projectname).wsgi
 22 
 23 # 和nginx通信地址:端口
 24 socket = 127.0.0.1:10000
-- INSERT --  
```





