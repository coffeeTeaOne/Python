Web应用介绍   windows创建虚拟环境,  Mac创建虚拟环境, 



### web应用



#### 为什么使用web 应用

* 基于浏览器访问, 浏览器普及率高
* 使用方便, 不用单独安装客户端
* 更新不用用户更新版本, 直接后台升级后,用户可以直接使用



#### 使用过程

* 输入URL --> 询问IP地址(DNS) , 返回IP地址 --> HTTP通信 (反向代理  -- > web服务器)--> 执行web应用  --> 过响应返回内容
* url 统一资源定位符
* 域名  与
* DNS 域名解析服务器
* IP地址 网络上的主机的身份标识
* HTTP 超文本传输协议, 应用层协议
* 反向代理
* Web 服务器
* Nginx 高性能Web 服务器 , 也可以做反向代理, 负载均衡, HTTP缓存



#### 发展过程

- 早期: 手动编辑, 修改HTML页面

- 发展: 用程序自动生成动态网站: 最早 - CGI技术(公共网关接口)

  - CGI - 代码大量重复, 总体性能较为底下

  - PHP  ASP  GSP 出现加速了动态网站的生成

     

#### Python Web 开发框架

- DJango
- Flask / Django / Tornado / Pyramid / Bottle(\*) / Web2py / web.py(\*)



#### Django概述

高内聚, 低耦合

- 使用了MVC架构  --  MVVM(改进版MVC)
  - model controller view
  - 做到数据与数据显示解耦合, 同一组数据以不同的形式显示
  - Django 里叫做 MCT,
- 快速上手
  - 传统 - 慢
  - 敏捷开发 - 增量迭代是开发, 快速出产品



#### Django 2.0

- 应用范围最广的是 1.11 可以兼容. 2.x 和 3.x
- 最新为 2.0 不兼容python 2.x



#### windows cmd创建Django虚拟环境

早期版本需要先安装包 `pip install virtualenv` 然后用virtualenv来安装虚拟环境

1. 检查版本

2. `python -m venv name`  venv 创建虚拟环境的模块, 创建虚拟环境

3. 激活虚拟环境  cd name/Scripts      输入`activate.bat`激活   反激活`deactivate.bat`

4. 激活后  ` pip install django==1.11`指定版本, 不写(==), 会安装最新版本

5. `django-admin --version  `检查django版本

6. 返回用户主目录, 在用户主目录创建项目
   - `django-admin startproject hello_django`
   - cd hello_django  查看内容   里面有个包  hello_django

7. `python manage.py runserver`启动服务器, 输入地址可查看是否启动成功

   - 更改设置 - settings.py 106 语言 zh-hans

   ```python
   LANGUAGE_CODE = 'zh-hans'   # 语言
   
   TIME_ZONE = 'UTC'
   
   USE_I18N = True    # 国际化  internationalization
     
   USE_L10N = True    # 本地化 locallization
   
   USE_TZ = True      # 
   ```

   



#### Linux, 和Mac基本相同

mkdir a

cd  a

python3 -m venv venv  创建虚拟环境

source venv/bin/activate  激活虚拟环境

更新pip

pip install django

django-admin startproject

python manage.py runserve 0.0.0.0:80 在80端口自动启动

