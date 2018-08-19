

day01

[TOC]



---

### 创建项目

#### 创建干净的虚拟环境

- 安装  pip install virtualenv
- virtualenv --no-site-package your_venv
- 激活虚拟环境  activate
- pip install django==1.11
- pip install pymysql
- pip freeze 查看pip安装的包(当前环境)   pip list  查看已安装的包(当前)
- django-admin startproject your_project .  加点号
- python manage.py startapp your_app




#### 加载项目

==配置环境==

出现控制台没响应, 更换端口

- settings--project Interpreyer - 加载创建的环境中的python.exe所在的文件

  ​




配置快捷启动

- 添加项目 manage.py文件, 并且加参数 runserver + port



==DEBUG==

- 可以在后台看POST GET 的参数
- 只要是requeset请求都会有user参数



==反向解析==

- 解析url,进入链接

- src='{% url 'namespace:name' %}'
- src = '/app/left/'

==静态解析==

- 加载静态文件
- {% load static %}
- \<img src="{% static 'image/xxx.jpg' %}">

#### 重定向

```python
from django.http import HttpResponseRedirect

通过返回  return HttpResopnseRedirect('/index/')   # 加上要重定向的路径,可以实现重定向

# 也可以 使用命名空间重新进行定向  通过reverse() 对url 的 name 进行解析 
HttpResponseRedirect(resverse('app:grade'))   # 'namespace:name'
# 还可以在其中添加参数
HttpResponseRedirect(resverse('index'[, args=(), kwargs={'a':1, 'b':2}]))
```

#### get_or_create()

- 如果查询有就返回查询的内容, 如果没有就将要查询的内容加入数据库, 相当于创建了新的对象
- 有 - 返回的是 True   没有 - 返回 False 同时创建对象    返回的是一个tuple



##### filter()和get() 获取对象的区别

- get()  拿不到对象会抛出异常, 一定要确定能够获得唯一对象
- filter()  拿不到元素会返回一个空的容器  \<Queryset [ ]>
  - 可以进行取值操作-切片-`first()   /   last()    [:1]`



### 分页操作,  Paginator

获取到的是一个Quserset

- 通过 page 获取 paginator对象   `page.paginator`

- pages.has_next  是否有下一页
- pages.has_provious  是否有上一页
- pages.next_pages_number 下一页的页码
- pages.previous_pages_number 上一页的页码

- {% for grade in pages %}   可以得到分页后一页中的所有grade对象
- pages.paginator.num_pages   获取最后一页页码

```python
from django.core.paginator import Paginator

def grade(request):
    if request.method == 'GET':
        page_num = request.GET.get('page_num', 1)
        grade_list = Grade.objects.all()
        paginator = Paginator(grade_list, 3)  # 将得到的所有对象按照3个一组分页
        pages = paginator.page(int(page_num))  # 根据页码得到对应的页面内容
        return render(request, 'grade.html', {'grade_list':grade_list, 'pages':pages})
```



```html
<ul>
    <li><a href="{% url 'app:grade' %}">首页</a></li>
    
    {% if pages.has_previous %}
    	<li><a href='{% url "app:grade" %}?page_num={{ pages.previous_page_number }}'> 上一页</a></li>
    {% endif %}
    
    {% for i in pages.paginator.page_range %}  <!--反向获取pages的paginator对象,对其进行操作-->
    	<li><a href="{% url 'app:grade' %}?page_num={{ i }}">{{ i }}</a></li>
    {% endfor %}
    
    {% if pages.has_next %}
    	<li><a href="{% url 'app:grade' %}?page_num={{ pages.next_page_num }}">
            下一页</a></li>
    {% endif  %}
    
    <li>当前页码-{{ pages.num }}</li>
    
    <li><a href="{% url 'app:grade' %}?page_num={{ pages.paginator.num_pages }}">尾页</a></li>
</ul>	
```

##

### html中的==frame==和==frameset==

- 将首页划分为框状

```html
<frameset> 元素被用来组织一个或者多个 <frame> 元素。每个 <frame> 有各自独立的文档。

<frameset> 元素规定在框架集中存在多少列或多少行，以及每行每列占用的百分比/像素。

注释：如果您希望验证包含框架的页面，请确保 <!DOCTYPE> 被设置为 "HTML Frameset DTD" 或者 "XHTML Frameset DTD" 。
```





==处理模型重复问题==Duplicate entry



#### ==模板的过滤器==

```python
在模板页面中使用 过滤器

    {{grade.gname|upper}}     大写
    {{grade.g_time|date:'Y-m-d h:m:s'}}   格式化输出
    add:1  加一
    add:-1   减 1
    lower  小写
    title  首字母大写
```

[更多过滤器介绍]((https://www.cnblogs.com/cpc-dingyi/p/5897101.html))