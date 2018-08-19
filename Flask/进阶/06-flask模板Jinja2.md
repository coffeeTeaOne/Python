##### flask模板Jinja2

---



Flask使用的是Jinja2模板引擎, Jinja2是模仿Django的模板引擎而开发的



#### Jinja2优点

-   速度快,广泛应用
-   HTML设计与后端分离
-   非常灵活,快速和安全
-   可以进行控制,继承等高级功能



#### 结构标签

-   block
-   主要用于在基础模板中定义 模块来在子页面中进行继承和扩展, 填充
-   避免的重复书写相同的结构, 是代码更加简洁

```python
{% extends 'xxx.html' %}

{% super() %} # 继承后保留原来基础模板中的内容
```

##### 宏定义

-   marco
-   可以传入参数, 也可以不传参数, 直接进行方法的调用执行

```python
{% macro func(name) %}
	{{ name }}
{% endmacro %}
```



定义一个function.html中定义一个方法：

```python
{% macro create_user(name) %}
    创建了一个用户:{{ name }}
{% endmacro %}
```

在index.html中引入function.html中定义的方法

```python
{% from 'functions.html' import create_user %}

{{ create_user('小花') }}
```



#### 模板语法

##### 变量

`{{ var }} ` 用两个大括号引起的  和 django模板用法基本相同

-   主要接收来自视图模板的参数, 以及前面所定义的参数, 如果变量不存在会默认忽略,不会报错

##### 标签

`{% tag %}{% endtag%}`

-   用来对模板进行逻辑的控制, 可以使用外部表达式, if  for  等
-   可以进行宏定义



##### 循环

-   `{% for item in items %}{% endfor %}`

##### loop

```python
loop.first   第一次循环 , {{ if loop.first }}  返回值是 True或False
loop.last  最后一次循环
loop.index   循环的次数
loop.revindex  反向的循环次数
```

##### 过滤器

-   `{{ 变量|过滤器|过滤器... }}`

```python
capitalize 首字母大写
lower 全小写
upper 全大写
title  第一个字母大写
trim  去除字符串前后的空格
reverse  反转
striptags  渲染之前, 将字符串中的标签去掉, 只保留字符串,忽略样式
safe  将字符串中的样式渲染到页面
last 最后一个字母
length   字符串的长度
```



#### 基础模板与静态文件配置

基础模板==base==

-   只有基本的样式和 block占位符

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>
        {% block title %}
        {% endblock %}
    </title>
    <script src="https://code.jquery.com/jquery-3.2.1.min.js"></script>

    {% block extCSS %}
    {% endblock %}
</head>
<body>

{% block header %}
{% endblock %}

{% block content%}
{% endblock %}

{% block footer%}
{% endblock %}

{% block extJS %}
{% endblock %}

</body>
</html>
```

==base_main==

-   在基础的 base 模板的基础上增加 css和js的公共的部分(header,footer)

```python
{% extends 'base.html' %}

{% block etxCSS %}
	<link rel='sytlesheet' href={{ url_for("static", filename="css/main.css") }}>
{% endblock %}
```



##### 静态文件的配置

第一种方式

```python
<link rel='stylesheet' href='/static/scc/index.css'>
```

第二种

```python
<link rel='stylesheet' href="{{ url_for('static', filename='css/index.css') }}">
```

