day-05-编辑班级,API数据格式重构, url请求过滤

[TOC]

---





###1. API & Ajax

postman 模拟登陆

- username   /   password    /   csrf_token(headers)



API接口, 页面编辑

- 页面跳转编写视图, 跳转后通过API接口发送Ajax请求, 进行 GET POST PUT PATCH DELETE操作
- GET  请求不需要csrf_token     修改参数的请求都需要加 令牌参数

```javascript
# API  Ajax使用

$(docment).ready(function () {
    var path = location.href;   # 本地的地址
    var num = path.split('=')[1];
    var csrf = $('input[name='csrfmiddlewaretoken']').val();
    $.ajax({
		url: '/API/' + num + '/',
        type: 'PATCH',  # PUT GET POST DELETE
        dataType: 'json',
        data: {},   # 传输的数据, 
        headers: {'X-CSRFToken': csrf},
        success: function (msg) {},  #msg为API接口返回的数据
        error: function (msg) {},
	});
});

# 简写
$.get(API, function (msg) {})
$.post(API, data, function(msg) {})
```





### 2. 重构API返回数据

-   重写`JSONRenderer`,设置返回的数据格式, 自定义类, 继承 JSONRenderer
-   早settings.py中更改默认的 render路径
-   serializer.py    重写 do_update() 方法
-   views 中重写 update() 方法


```python
class CustomJsonRender(JSONRenderer):
    # 重写render方法
    def render(self, data, accept_media_type=None, renderer_context=None):
        if renderer_context:
        	if isinstance(data, dict):
                code = data.pop('code', 0)
                msg = data.pop('msg', 'OK')
            else:
                code = 0
                msg = 'OK'
            res = {
            	'code': code,
                'msg': msg,
                'data': data
            }
            return super().render(res, accept_media_type, renderer_context)
        else:
            return super().render(data, accept_media_type, renderer_context)
```

```python
'DEFAULT_RENDERER_CLASSES': ('utils.functions.CustomJsonRenderer',)
```

![pdat](assets/update.png)

![o_updat](assets/do_update.png)





### 3. 通过URL进行数据的筛选

-   url 格式  /app/api/student/?name_like=wang&s_shuxue_max=90

**方法1, 不推荐, 了解**

-   通过重写viewsets.GenericViewSet 中的get_queryset() 方法

```python
def get_queryset(self):
    query = self.queryset
    return query.filter()  # 这里写筛选条件, 相当于二次筛选
```



**方法2**

通过在views中更改默认的filter_class 来实现自定义的数据筛选

-   pip install django_filter

-   djangorestframework==3.4.6, 高版本不支持filter函数

-   seting  增加  rest [配置 

    ```python
    	'DEFAULT_FILTER_BACKENDS': (
            'rest_framework.filters.DjangoFilterBackend',
            'rest_framework.filters.SearchFilter',   # 添加自定义的search
        )
    ```

-   app中新建自定义的 filter.py文件

```python
import django_filters
from rest_framework import filters
from app.models import Studnet

class StudnetFilter(filters.FilterSet):
    # 自定义查找字段
    s_name_like = django_filters.CharFilter('s_name', lookup_expr='contains')
    s_shuxue_max = django_filters.Charfilter('s_shuxue', lookup_expr='gte')
    
    class Meta:
        model = Student
        fields = ('s_name', 's_shuxue',)
```

-   views.py 指定 filter_class

​    `filter_class = StudentFilter`

 

### 4. 配置日志

创建 log 目录 存放日志

```css
DEBUG：用于调试目的的底层系统信息

INFO：普通的系统信息

WARNING：表示出现一个较小的问题。

ERROR：表示出现一个较大的问题。

CRITICAL：表示出现一个致命的问题。
```

formatter格式化字符

![52790861560](assets/1527908615605.png)



配置日志

```python
LOGGING = {
    'version': 1,  # 只能为1
    # disable_exsiting_loggers 键 默认值为 True, 日志中的所有 logger 都会被禁用
    # logger 禁用与删除不同, logger 任然存在  但是会将传递过来的信息丢弃, 也不会传递给上一层logger
    'disable_existing_loggers': False,
    'formatters': {
        'verbose': {
            'format': '%(levelname)s %(asctime)s %(module)s %(process)s %(thread)s %(message)s'
        },
        'simple': {
            'format': '%(levelname)s %(message)s'
        },
    },
    'handlers': {
        'stu_handlers': {
                # 如果loggers 的处理级别小于 handlers 的处理级别, 则 handlers 忽略该信息
                'level': 'DEBUG',
                # 指定文件的类型为 RotatingFileHandler, 当日志文件大小超过 maxBytes以后, 就会自动切割
                'class': 'logging.handlers.RotatingFileHandler',
                # 文件输出地址
                'filename': '%s/log.txt' % LOG_PATH,
                # 使用哪一个日志格式化配置
                'formatter': 'verbose',
                # 指定日志文件的大小   5M
                'maxBytes': 1024 * 1024 * 5
        },
    },
    'loggers': {
        'console': {
            'handlers': ['stu_handlers'],
            'lever': 'INFO',   #  日志级别, 暂时使用较低级别测试日志
            'propagate': False,  # 0 表示输出日志, 但消息不传递, 1 输出日志, 同时传递消息到更高的级别, root为最高级别

        }
    }

}
```



```python
import logging
import django.utils.log
import logging.handlers

logger = logging.getLogger('django.request')  # 日志
```

![52790840964](assets/1527908409649.png)







####  完整示例

```python
 seetings.py  添加
""" 日志配置 """

LOG_PATH = os.path.join(BASE_DIR, 'log')
# 不存在目录就配置目录
if not os.path.isdir(LOG_PATH):
    os.mkdir(LOG_PATH)

LOGGING = {
    'version': 1,
    'disable_existing_loggers': True,
     'formatters': {
        'verbose': {
            'format': '%(levelname)s %(asctime)s %(module)s %(process)s %(thread)s %(message)s'
        },
        'simple': {
            'format': '%(levelname)s %(message)s'
        },
    },
    'filters': {
    },
    'handlers': {
        'mail_admins': {  # 不能加 encoding
            'level': 'ERROR',
            'class': 'django.utils.log.AdminEmailHandler',
            'include_html': True,
        },
        'default': {
            'level': 'DEBUG',
            'class': 'logging.handlers.RotatingFileHandler',
            'filename': '%s/default.txt' % LOG_PATH,  # 日志输出文件
            'maxBytes': 1024 * 1024 * 5,  # 文件大小
            'backupCount': 5,  # 备份份数
            'formatter': 'verbose',  # 使用哪种formatters日志格式
            'encoding': 'utf8',
        },
        'error': {
            'level': 'ERROR',
            'class': 'logging.handlers.RotatingFileHandler',
            'filename': '%s/error.txt' % LOG_PATH,
            'maxBytes': 1024 * 1024 * 5,
            'backupCount': 5,
            'formatter': 'verbose',
            'encoding': 'utf8',
        },
        'console':{  # 不能加 encoding
            'level': 'DEBUG',
            'class': 'logging.StreamHandler',
            'formatter': 'simple',
        },
        'request_handler': {
            'level': 'DEBUG',
            'class': 'logging.handlers.RotatingFileHandler',
            'filename': '%s/request_handler.txt' % LOG_PATH,
            'maxBytes': 1024 * 1024 * 5,
            'backupCount': 5,
            'formatter': 'verbose',
            'encoding': 'utf8',
        },
        'scprits_handler': {
            'level': 'DEBUG',
            'class': 'logging.handlers.RotatingFileHandler',
            'filename': '%s/script_handler.txt' % LOG_PATH,
            'maxBytes': 1024 * 1024 * 5,
            'backupCount': 5,
            'formatter': 'verbose',
            'encoding': 'utf8',
        }
    },
    'loggers': {
        'django': {
            'handlers': ['default', 'console'],
            'level': 'DEBUG',
            'propagate': False
        },
        'django.request': {
            'handlers': ['request_handler'],
            'level': 'DEBUG',
            'propagate': False,
        },
        'scripts': {
            'handlers': ['scprits_handler'],
            'level': 'INFO',
            'propagate': False
        },
        'sourceDns.webdns.views': {
            'handlers': ['default', 'error'],
            'level': 'DEBUG',
            'propagate': True
        },
        'sourceDns.webdns.util': {
            'handlers': ['error'],
            'level': 'ERROR',
            'propagate': True
        }
    }
}
```





#### 自定义错误页面

*   DEBUG=False      True看不见

![1527842511658](assets/1527842511658.png)



#### 0

student.0.s_name

![1527842869637](assets/1527842869637.png)



#### 注释

![1527842952755](assets/1527842952755.png)

