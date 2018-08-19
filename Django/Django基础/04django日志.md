Django-Day03



HTTP协议

1.1 -->  2.0

请求行 请求头  空行 请求体

WireShark 抓包和协议分析工具, 前身是 Ethereal

只要将网卡设置为混杂模式, 可以截获局域网所有数据



MIME类型

Multipurpose  Internet Mail Extensions 多用途互联网邮件扩展



#### [Django 记录日志](https://docs.djangoproject.com/zh-hans/2.0/topics/logging/#examples)

调试阶段设置为 DEBUG   项目上线   日志登记为  ERROR

```python
# settings.py,  文件输出日志
LOGGING = {
    'version': 1,
    'disable_existing_loggers': False,
    'handlers': {
        'file': {
            'level': 'DEBUG',
            'class': 'logging.FileHandler',
            'filename': '/path/to/django/debug.log',
        },
    },
    'loggers': {
        'django': {
            'handlers': ['file'],
            'level': 'DEBUG',
            'propagate': True,
        },
    },
}

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



request.get_host()  请求主机  port- 端口  full_path()

request.META    得到的是字典, 可以得到请求头

​    	request.GET  

filter() 得到的是以QuerySet对象,  get 得的是对象本身



##### 惰性查询

- 不是必须要数据时, 才会发送SQL语句来进行查询
- 节省服务器内存的,   延迟加载,   节省了空间浪费了时间

Queryset 惰性查询    all() / filter() 得到的是Queryset



```python
<td><button type="button" class="btn btn-danger btn-xs"><a href="{% url 'ddel' %}?no={{ dept.no }}">删除</a></button></td>

# 过滤,  排序,  切片, 连接查询
def emps(request):
    dno = request.GET['no']
    emp_list = list(Emp.objects.filter(dept__no=dno).orderby()[0:2].select_related('dept'))  # 联查, 关联对象字段名, models的class Dept
    ctx = {
        'emp_list': emp_list,
        'dept_name': emp_list[0].dept.name
    } if len(emp_list) > 0 else {}
    return render(request, 'emp.html', context=ctx)
```





尽量避免使用硬代码(hard code)





{{ 占位符 }}  {% 分支循环, url %}



EAFP 优于LBYL

try 优于 if





##### 正则表达式命名捕获组

```python
(?<name>exp)   Python 中要加P (?P<name>exp)
```



##### url 后缀

没有后缀的URL 

Django 1.x 和 2.x 做法是不同的

```python
#1.x      pattern
from django.conf.urls import url
urlpatterns = [url(),
              url('depts/emps/(?P<no>[0-9]+)', views.emps, name='empsindept')   # 可以写正则表达式
              ]

在 views.py中 , 可以直接获得参数
def func(request, no):
    pass
```

##### 2.x

```python
# 2.x
urlpatterns = [path('dept/emps/<int:no>'), ]

```





#### Ajax删除

```python

```





作业

Django  1.11  

新建项目  输入框 -  车牌号,  按钮 - 点击按钮查询有没有违章记录 



记录编号, autofield

车牌号码

违章原因

违章日期

处罚方式

是否受理



db_column = 'dno'  在数据库中的名字

db_index=''   创建索引



自参照完整性   

mgr = models.ForeignKey('self', null=True, blank='True', on_delete =Models.set_null)   参照自己的主键