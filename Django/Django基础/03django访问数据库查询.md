Django123



后端渲染

- 服务器渲染

前端渲染

- 浏览器渲染



OA - 办公自动化系统

#### urls 中1.x和2.x的区别

Django1.x - url()

Django2  -   path() / re_path()  可以写正则



#### 配置静态资源 settints.py

添加静态资源目录

自己创建文件目录

​	`STATICFILES_DIRS = [os.path.join(BASE_DIR, 'static')]`



#### 连接数据库, 管理数据库, 访问数据

```python
__exact 精确等于 like ‘aaa’ 
__iexact 精确等于 忽略大小写 ilike ‘aaa’ 
__contains 包含 like ‘%aaa%’ 
__icontains 包含 忽略大小写 ilike ‘%aaa%’，但是对于sqlite来说，contains的作用效果等同于icontains。 
__gt 大于 
__gte 大于等于 
__lt 小于 
__lte 小于等于 
__in 存在于一个list范围内 
__startswith 以…开头 
__istartswith 以…开头 忽略大小写 
__endswith 以…结尾 
__iendswith 以…结尾，忽略大小写 
__range 在…范围内 
__year 日期字段的年份 
__month 日期字段的月份 
__day 日期字段的日 
__isnull=True/False 
__isnull=True 与 __exact=None的区别
```



setting.py中将DATABASE设置改为mysql

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',   # 引擎
        'NAME': 'oa',
        'HOST': 'localhost',
        'port': 3306,
        'USER': 'root',
        'PASSWORD': '123456'
    }
}
还需要将__init__.py中将pymysql设置, 在 setting.py同级目录下
import pymysql

pymysql.install_as_Mysqldb
```



在  templates中创建网页模板

在 urls中添加path

在views.py中创建渲染,

在 app --> models中可以创建数据库的表格参数, 直接通过

- `python manage.py makemigrations yourApp`创建静态模板
- `Python manage.py migrate`加载数据



app --> admin.py 后台显示

```python
from django.contrib import admin

# Register your models here.
from hrs.models import Dept, Emp


class DeptAdmin(admin.ModelAdmin):

    list_display = ('no', 'name', 'location')
    ordering = ('no',)  #元组 前面加 '-no' 为降序, 'no' 为升序


class EmpAdmin(admin.ModelAdmin):

    list_display = ('no', 'name', 'job', 'sal', 'comm', 'dept_id', 'mgr')
    search_fields = ('name',)
    ordering = ('dept_id',)


admin.site.register(Dept, DeptAdmin)
admin.site.register(Emp, EmpAdmin)

```



```python
templaes --> index.html
<!DOCTYPE html>
{#{% load staticfiles %}#}
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <h1>Hello, Django!</h1>
    <hr>
    {% load static %}
    <img src="{% static 'images/aaa.jpg' %}" alt="图片">

</body>
</html>
```



#### 连接数据库

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

python manage.py migrate   # 在数据库创建Django需要的表
python mansge.py flush      # 删除表
```

#### 建模型

app -> models.py

```python
from django.db import models

# Create your models here.
# 一个类对应一张表, 类的属性就是表的列, 创建的对象就对应着表里面的一条数据记录
# 对象关系映射   ORM
#

# 继承 models.Model
class Dept(models.Model):
    no = models.IntegerField(primary_key=True)
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



#### Django管理员平台

创建超级管理员

- `python manage.py createsuperuser`

注册  在 app -> admin 里加注册语句

```python
from django.contrib import admin

# Register your models here.
from hrs.models import Dept, Emp

admin.site.register(Dept)
admin.site.register(Emp)
```







最小惊讶原则

? 是中文用拉丁1 语系编码,









#### 对表进行增删改查

python manage.py shell 开启交互式环境

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



优秀部门, 加 * 图片, 

BooleanField , default=0, 

修改模型生成迁移



部门超链接, 点击后可以进入该部门所有员工的页面

```python
< a href='/hrs/emps?dno={{dept.no}}'> </a> # 筛选条件, 请求参数

def emps(request):
    dno = request.GET['dno']
    dept = Dept.Objects.get(pk=dno)
    Emp.object.filter(dept=dept)
```



删除按钮, 执行删除





git  忽略文件,    git 仓库创建文件  touch .gitignore, 添加需要忽略的文件

git pull request  ''   发通知

















