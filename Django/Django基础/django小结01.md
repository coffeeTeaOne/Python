django小结

---

### Pyhton 的 web开发框架

- **Flask** / **Django** / Tornado / Pyramid / **Bottle** / Web2py / **web.py** / 



#### Django

高内聚, 低耦合

- 使用MVC架构      (model controller view)
- 做到数据与数据的显示解藕和, 同一组数据可以以不同的形式显示
- Djago可以快速上手, 快速出产品



#### Django开始项目步骤

1. 创建虚拟环境`python -m venv venv`

2. 激活虚拟环境 'activate'   Linux--> `source ./activate`

3. 安装django `pip install django`  默认安装最新版, ==1.11可以安装指定的版本

4. 开启项目 ` django-admin startproject your_django_project`

5. 生成app `python manage.py startapp your_app`

6. 如果需要连接数据库, 还需安装数据库的包- 例如连接MySQL要安装`pymysql`

   

   **Mac创建Django虚拟环境**

   ```python
   命令行输入:
   mkdir hello_django         创建项目目录
   cd hello_django            切换到项目目录
   python3 -m vevn hd_vevn    使用venv模块创建虚拟环境, 目录名hd_venv
   source hd_vevn/bin/activate  激活虚拟环境
   python -m pip install --update pip    更新pip到最新版
   pip install django        pip安装django
   django-admin --version    通过安装django时安装的脚本工具django-admin检查django版本
   django-admin startproject hello_django  . 加点号, 可以将文件生成在当前目录下
   cd hello_django  查看内容   里面有个包  hello_django
   python manage.py runserver启动服务器, 输入地址可查看是否启动成功
   更改设置 - settings.py 106 语言 zh-hans
   ```



#### Django项目文件目录作用

- 项目目录
  - 存放的是项目的设置文件, 整个项目的Url链接设置
  - `settings.py / urls.py / __init__.py`
- 应用目录  `your_app`
  - 目录包含文件-->`__init__.py / admin.py /apps.py / models.py / urls.py / views.py / test.py `
  - 可以在里面创建model , 和后台注册, 设置url , 最主要就是views.py, 控制页面模板的渲染
  - `python manage.py makemigrations yourApp`创建静态模板
  - `Python manage.py migrate`加载数据
- static 存放项目的静态文件
  - 需要在settings.py中添加`STATICFILES_DIRS = [os.path.join(BASE_DIR, 'static')]`
  - images / css / js / 都可以存放在这里
- templates目录文件存放项目的模板页面
  - 也要添加路径, 在`TEMPLATES`中添加`'DIRS': [os.path.join(BASE_DIR, '	templates')]`



#### 模板页面的占位符

```python
{% load staticfiles %}
{{ static 'image/a.jpj' }}   # 静态文件占位

{{ gretting }}   # 普通字符占位
{% url 'url_name' %}   # url占位

# 循环和分支
{% for _ in list  %}
{% endfor %}

{% if conditon %}
{% else %}
{% endif %}
```



### url, 同一资源定位, 2.x版本写法

1.x写法有很多不同

```python
from django.urls import path, include
from django.contrib import admin

from your_app import views 

# 在your_project文件设置
urlpattern = [
    path('', views.index)
    path('admin/', admin.site.urls)
    path('your_app/', include('your_app.urls'))
]

# 在 your_app中设置urls
# 可以给每个功能一个 url , 通过对应 的url实现指定的功能

# 1.x 和 2.x 中 url 的区别
1.x - > 
urlpatterns=[  
    url(r'^$',views.index,name='index'),  
    url(r'^topics/$',views.topics,name='topics'),  
    url(r'^topics/(?(?P<topic_id>\d+)/)?$',views.topic,name='topic'), 
    ] 

2.x - >
urlpatterns=[  
    path('',views.index,name='index'),  
    path('topics/',views.topics,name='topics'), 
    path('topics/<int:no>',views.topics,name='topics'), 
    re_path(r'^topics/(?:(?P<topic_id>\d+)/)?$',views.topic,name='topic'),  
]  
```



#### 控制器 views.py

- 对页面渲染

```python
def index(request):
    ctx = {'占位符': '设置内容'}
    return render(request, 'index.html', ctx)
```



#### [Django 记录日志](https://docs.djangoproject.com/zh-hans/2.0/topics/logging/#examples)

调试阶段设置为 DEBUG   项目上线   日志登记为  ERROR

```python

# 控制台输出
import os

LOGGING = {
    'version': 1,
    'disable_existing_loggers': False,
    'handlers': {
        'console': {
            'class': 'logging.StreamHandler',
        },
    },
    'loggers': {
        'django': {
            'handlers': ['console'],
            'level': os.getenv('DJANGO_LOG_LEVEL', 'INFO'),
        },
    },
}
```



#### 连接数据库的设置

```python
setting.py,
pip install pymysql  # 安装mysql
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',   # 引擎
        'NAME': 'oa',
        'HOST': 'localhost',
        'port': 3306,
        'USER': 'root',    # 本地连接, 远程连接新建用户, 给与权限
        'PASSWORD': '123456'
    }
}

项目  - __init__.py
import pymysql

pymysql.install_as_MySQLdb()   #将mysql当做mysqldb

python manage.py makemigrations /  migrate   # 在数据库创建Django需要的表
python mansge.py flush      # 删除表
```



#### 建模型models

app -> models.py

```python
from django.db import models

# Create your models here.
# 一个类对应一张表, 类的属性就是表的列, 创建的对象就对应着表里面的一条数据记录
# 对象关系映射   ORM
#

# 继承 models.Model
class Dept(models.Model):
    no = models.IntegerField(primary_key=True)  # 不写主键会自动增加一列作为主键
    name = models.CharField(max_length=20)  # 默认不为空 not null
    location = models.CharField(max_length=20)


class Emp(models.Model):
    no = models.IntegerField(primary_key=True)
    name = models.CharField(max_length=20)
    job = models.CharField(max_length=10)
    sal = models.DecimalField(max_digits=7, decimal_places=2)
    comm = models.DecimalField(max_digits=7, decimal_places=2, null=True) # null = True  表示可以放空值
    dept = models.ForeignKey(Dept, on_delete=models.PROTECT)   #放在 1对 多的 多的一方    在删除时保护  , null = True on_delete = models.setnull
      
```

`python manage.py makemigrations hrs `    # 生成数据模型

`python manage.py migrate`   # 数据迁移

会在数据库表 django_migrations中产生一条迁移记录



##### 实际开发都使用两个一对多来替代多对多



#### Django管理员平台注册

创建超级管理员

* `python manage.py createsuperuser`

注册  在 app -> admin 里加注册语句

```python
from django.contrib import admin

# Register your models here.
from hrs.models import Dept, Emp

admin.site.register(Dept)
admin.site.register(Emp)
```



#### 对数据库表进行增删改查

`python manage.py shell `开启交互式环境

```python
from hrs.models import Emp

Emp.objects.all()   # 查看所有
dept = Dept(no='90', name='研发2部', location='武汉')
>>> dept.no
'90'
>>> dept.save()   # 保存到数据库
dept.delete()  # 删除
Dept.objects.get(pk=20)  # 主键

#筛选
dept = Dept.objects.filter(name='运维')
dept.name
dept.no

Dept.objects.filter(name__icontains='运维')   # 模糊查询, i 忽略大小写, list将结果转为一个列表

Dept.object.filter(no__gte=30)  # 查找 no 大于 30   gte 大于等于

Dept.objects.all().order_by('-no')[0:3]  # 降序, 切片
Dept.objects.all().order_by('-no')[2:4]  

Dept.objects.raw('select name from tb_dept')   # 原始操作

emp = Emp.objects.get(pk=7800)
emp.dept.name      # 可以将其另一张关联表的内容也差出来

#QuerySet 负数索引不被支持

```

