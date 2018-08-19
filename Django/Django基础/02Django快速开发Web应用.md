Django快速开发Web应用



#### Django快速开发步骤

- mkdir  DIR_NAME   在创建的文件目录下创建虚拟环境 
  - python3 -m venv VENV_NAME
  - 激活虚拟环境 - source ./activate
- python -m pip install --upgrate pip 升级pip 
- pip install Django    安装Django
- django-admin --version  查看版本
- django-admin startproject PROJECT_NAME .  在最后加点号, 可以让项目创建在当前目录下
- python manage.py runserver 0.0.0.0:80启动服务器, 指定端口



#### PROJECT_NAME/ views.py

```python
from django.shortcuts import render  #渲染函数
from django.http import HttpResponse
from django.template import Template, Context
from django.template.loader import get_template

from random import randrange

from datetime import datetime


def home(request):
	fruit_list = ['苹果', '草莓', '蓝莓', '西瓜', '苹果']
	start = 0
	end = randrange(len(fruit_list))
	fruits = fruit_list[start:end]
	ctx = {
	'greeting': '你好世界',
	'current_time': datetime.now(),
	'num': len(fruits),
	'fruits': fruits
	}
	# 第二个参数是模板页面(路劲在settings.py)
	# 第三个参数是一个字典(替换占位符)
	return render(request, 'index.html', ctx)

# def home(request):
# 	with open(r'C:\Users\Administrator\hell_django\templates\index.html', encoding='utf-8') as f:
# 		t = Template(f.read())
# 	index = randrange(0, len(fruit_list))
# 	ctx = Context({
# 	'greeting': '你好世界',
# 	'current_time': datetime.now(),
# 	'fruit': fruit_list[index]
# 	})
# 	return HttpResponse(t.render(ctx))     # 渲染


# fruit_list = ['苹果', '草莓', '蓝莓', '西瓜', '苹果']
# # Create your views here.
# def home(request):
# 	html_str = '<h1>hello, django!</h1>'
# 	html_str += '<ul>\n'
# 	for _ in range(3):
# 		index = randrange(0, len(fruit_list))
# 		html_str += '\t<li>' + fruit_list[index] + '</li>\n'
# 	html_str += '</ul>'

# 	return HttpResponse(html_str)
# 

# def home(request):
# 	html = '北京时间: %s' % datetime.now()
# 	return HttpResponse(html)


```

#### hello_django -> settings.py / urls.py

- setting.py
  - 更改DEBUG  ['*']
  - 修改seetings.py  33行, installed_app    增加应用, 追加名字
  - DIR []

```python
from django.contrib import admin
from django.urls import path
from hrs import views

urlpatterns = [
    path('', views.home),
    path('admin/', admin.site.urls),
]
```

#### templates - > index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>首页</title>
</head>
<body>
	<h1>{{ greeting }}</h1>   <!-- 占位符-->
	<h3>{{ current_time }}</h3>
	<h3>今天推荐{{ num }}种水果:</h3>
	<hr>
	<ul>
		{% for fruit in fruits %}
		<li>{{ fruit }}</li>
		{% endfor %}
	</ul>
</body>
</html>
```



#### 写页面

- OA - office Auomation
- 创建应用`python manage.py startapp hrs` , 创建成功后会有一个对应的文件夹

```python
hrs 文件目录包含以下内容
__init__ 包
apps  应用
models 模型
tests 测试
views  控制器
admin 注册  
migrations 目录为数据库相关文件
```



stringIO 模块拼接字符串, write  value

[Django官方文档](https://docs.djangoproject.com/zh-hans/2.0/)



