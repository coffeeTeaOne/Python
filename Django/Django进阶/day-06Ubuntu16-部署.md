Ubuntu16-部署



部署2: 测试环境部署方式和线上环境nginx+uwsgi部署方式：

部署的服务器采用的是ubuntu16系统

#### 环境搭建:

1.  安装包

    sudo apt update

    ```
    apt install mysql-server mysql-client
    ```

2.  设置远程访问mysql

    a) 查找 mysql.conf 
    		find / -name mysql.cnf

    ```
    b）注释mysql.cof文件的bind_address
    	cd /etc/mysql/mysql.conf.d
    	vim mysqld.cnf

    c）切换mysql数据库
       use mysql;

       GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'root123' WITH GRANT OPTION;

       flush privileges; 

    d) 重启
       service mysql restart /status/ stop/ start
    ```

#### 在测试环境中部署方式：

1.  修改django的配置文件

    a）修改settings.py文件中的DEBUG=FALSE，ALLOWED_HOST=['*']

    ```
    b）修改urls.py

    	b1）url中加入以下配置
    	from django.views.static import serve
    	url(r'^static/(?P<path>.*)$', serve, {"document_root": settings.STATIC_ROOT}),
    	url(r'^media/(?P<path>.*)$', serve, {"document_root": settings.MEDIA_ROOT}),

    	b2）setting中
    	STATIC_ROOT = os.path.join(BASE_DIR, 'static')

    	b3）url中修改首页访问的地址
    	url(r'^$', views.home)
    ```

2.  修改首页的启动地址

    修改工程目录中的url ，并修改url(r'^$', views.home)

3.  安装pip3

    apt install python3-pip

4.  安装必备库

    pip3 install django==1.11
    	pip3 install pymysql
    	pip3 install Pillow

5.  查看进程

    netstat -lntp

6.  启动项目

7.  python3 manage.py runserver 0.0.0.0:80

#### 使用nginx+uwsgi配置django项目

1.  安装nginx

    sudo apt install nginx

2.  查看nginx的状态

    systemctl status nginx 查看nginx的状态
    	systemctl start/stop/enable/disable nginx 启动/关闭/设置开机启动/禁止开机启动
    	

    ```
    或者是如下命令：

    service nginx status/stop/restart/start
    ```


1.  安装uwsgi

    pip install uwsgi

2.  nginx的配置文件中加载自定义的nginx的配置文件

    vim /etc/nginx/nginx.conf
    	在server中加入以下配置：
    	include /home/app/conf/*.conf;


1.  配置自定义的nginx配置文件

    server {
    	    listen       80;
    	    server_name 47.92.164.198 localhost;

    ```
        access_log /home/app/log/access.log;
        error_log /home/app/log/error.log;

        location / {
            include uwsgi_params;
            uwsgi_pass 127.0.0.1:8890;
        }
        location /static/ {
            alias /home/app/day11axf0/static/;
            expires 30d;
        }

    }
    ```


1.  配置uwsgi，名称为uwsgi.ini

    [uwsgi]
    	# variables
    	projectname = day11axf0
    	newprojectname = day11axf
    	base = /home/app

    ```
    # config
    #plugins = python
    master = true
    #protocol = uwsgi
    processes = 4
    #env = DJANGO_SETTINGS_MODULE=%(projectname).settings
    pythonpath = %(base)/%(projectname)
    module = %(newprojectname).wsgi
    socket = 127.0.0.1:8890
    logto = %(base)/log/uwsgi.log
    ```

```
启动方式： uwsgi --ini uwsgi.ini

```

### 重点：

### 重点：

#### nginx + uWSGI + django的处理流程

*   首先nginx 是对外的服务接口，外部浏览器通过url访问nginx,
*   nginx 接收到浏览器发送过来的http请求，将包进行解析，分析url，如果是静态文件请求就直接访问用户给nginx配置的静态文件目录，直接返回用户请求的静态文件，
*   如果不是静态文件，而是一个动态的请求，那么nginx就将请求转发给uwsgi,uwsgi 接收到请求之后将包进行处理，处理成wsgi可以接受的格式，并发给wsgi,wsgi 根据请求调用应用程序的某个文件，某个文件的某个函数，最后处理完将返回值再次交给wsgi,wsgi将返回值进行打包，打包成uwsgi能够接收的格式，uwsgi接收wsgi发送的请求，并转发给nginx,nginx最终将返回值返回给浏览器。
*   要知道第一级的nginx并不是必须的，uwsgi完全可以完成整个的和浏览器交互的流程，但是要考虑到某些情况
    *   安全问题，程序不能直接被浏览器访问到，而是通过nginx,nginx只开放某个接口，uwsgi本身是内网接口，这样运维人员在nginx上加上安全性的限制，可以达到保护程序的作用。
    *   负载均衡问题，一个uwsgi很可能不够用，即使开了多个work也是不行，毕竟一台机器的cpu和内存都是有限的，有了nginx做代理，一个nginx可以代理多台uwsgi完成uwsgi的负载均衡。
    *   静态文件问题，用django或是uwsgi这种东西来负责静态文件的处理是很浪费的行为，而且他们本身对文件的处理也不如nginx好，所以整个静态文件的处理都直接由nginx完成，静态文件的访问完全不去经过uwsgi以及其后面的东西。

uWSGI是一个Web服务器，它实现了WSGI协议、uwsgi、http等协议。Nginx中HttpUwsgiModule的作用是与uWSGI服务器进行交换。

