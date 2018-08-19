day03   模板 查找运算 权限用户角色

[TOC]

---

###1.模板

关键字  `{% extends 'base_main.html %}'`继承自的模板 `{% block title %}{% endblock %}`继承的块的位置  `{{ block.super }}`

- 定义基础模板 base.html

- 定义二级基础模板, 继承自上层 base.html, 加样式或者JS

- 在模板中定义 block模块

- {{ block.super }}  避免被覆盖

- 模板  

  ifequal  a  b   等于

  forloop.counter0 循环打印, 记录循环的次数 , 可以指定开始


```python
{% block extJS %}
    {{ block.super }}  # 避免被覆盖
<script src="/static/js/side.js" type="text/javascript"></script>
{% endblock %}
```

==base.html==

```python
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>
            {% block title %}
            {% endblock %}
        </title>
        {% block extCSS %}
        {% endblock %}
        {% block extJS %}
        {% endblock %}
    </head>

    {% block indexbody %}
        <body>
            {% block content %}
            {% endblock %}
            {% block footer %}
            {% endblock %}
        </body>
    {% endblock %}
</html>
```

==base_main.html==

```python
{% extends 'base.html' %}

{% block extJS %}
        <script type="text/javascript" src="/static/js/jquery.min.js"></script>
{% endblock %}
```

####\<a>\</a>标签的target属性  

- _blank  新窗口打开
- _self  默认值, 在相同的窗口打开
- _parent  在整个窗口打开
- framename  在指定的框架中打开, 在frame中指定 name=''

### 2.权限用户角色

用户 -角色  (一对多  一对一)

角色和权限  (多对多)



------

### [3.模型查询](https://blog.csdn.net/u014745194/article/details/74612947)

* ==DecimaField==(max_digits=None, decimal_places=None)
  * max_digits 最大总位数, decimal_place小数位数
* FloatField 浮点数
* DateField()
  * auto_now表示每次保存对象时, 会自动设置为该字段时间为当前时间
  * auto_now_add 只在创建时设置时间
* TimeField / DateTimeField
* FileField 上传文字字段

```python
# 精确查找
Model.object.filter(id=1)  # 具体属性查找 id__exact=1
# 模糊查找
Model.object.filter(name__contains='张飞')
Model.object.filter(name__endwith/startwith='value')
# 空查找
Model.object.filter(name__isnull=False)
# 范围查找
Model.object.filter(name__in=['', '', ''])
# 比较查找
Model.object.filter(age__gte=20)
# 日期
Model.object.filter(date__year__gte=1990)
Model.object.filter(date__gte=date(1990,1,1))
# 返回不满足条件的数据
Model.object.exclude(name='')
```

#### 返回不满足条件的数据exclude()

Model.object.==exclude==(name='')



* order_by  **排序**

```python
# 模型名为Book
Book.objects.all().order_by('id')
Book.objects.all().order_by('-id')  # 降序
```

* 聚合函数  sum count max min avg
* Book.objects.all().==aggregate==(Count('id'))    得到一个==字典==

```python
from django.db.modles import Sum, Count, Max, Min, Avg
Book.objects.all().aggregate(Count('id'))
Book.objects.all().aggregate(Sum('id'))

Book.objects.all().all.count()  # 统计总数
```

####F&Q

* F / Q
* F  用于类属性之间的比较
* Q 用于查询时加逻辑条件   & | ~

```python
from app.models import Grade, Student
from django.db.models import Q, F

def selectstu(request):
    grade = Grade.obgect.filter(g_name='python').first()
    # 通过班级反向查找学生, 写小写的模型名加 _ 如, student_set
    students = grade.student_set.all()
    # 查询语文比数学高10分的学生   F
    sutdents.filter(s_yuwen__gt=F('s_shuxue')+10)
    # 做 或 且 非(~)运算
    students.silter(Q(s_yuwen__gte=80) | Q(s_shuxue__gte=80))
    students.silter(Q(s_yuwen__gte=80) & Q(s_shuxue__gte=80))  # 且
    students.silter(~Q(s_yuwen__gte=80) | Q(s_shuxue__gte=80))  # 非
    
```

#### 关联查询

先要查找到对象 ,再通过类名小写来查找, *外键关联一般在多的一方*

* ==一类== 查询 ==多类==

  - 先要查找到对象

  * 一类对象.==多类名==小写_set.all()  # 查询所用的数据

  ```python
  # 外键在 card中
  >>> stu = Student.objects.filter(s_name='刘备').first()
  >>> stu.card_set.all()
  ```

  

* ==多==类的对象查询==一==类
  * 多类对象.关联==属性==.具体属性    # 查询多类的对象对应的一类对象

  * ```python
    #外键所在的类,通过外键查找关联的类
    card = Card.objects.filter(cd='a').first()
    card.s.s_name
    ```

    

* ==多==类对象查询==一==类对象==id==
  * 多类对象.关联==属性==_id

* 通过==多==类的条件查询==一==类的数据
  * 一类名.objects.filter(多类名小写\__多属性名__条件名=值)

* 通过==一==类的条件查找==多==类数据
  * 多类名.objects.filter(关联属性\__一类属性名__条件名=值)

```python
# 通过在role模型中设置的关联, 来调用role模型来查找User, 关联设置在Role模型中
Users.objects.filter(role__r_name__contains='主')
```

#### 自关联

```python
## 自关联,特殊的一对多的关系
对于地区信息、分类信息等数据，表结构非常类似，每个表的数据量十分有限，为了充分利用数据表的大量数据存储功能，可以可以设计成一张表，内部的关系字段指向本表的主键，这就是自关联的表结构。

#定义地区模型类，存储省、市、区县信息
class AreaInfo(models.Model):
    atitle=models.CharField(max_length=30)

    # 关系属性使用self指向本类，要求null和blank允许为空，因为一级数据是没有父级的。

    aParent=models.ForeignKey('self',null=True,blank=True)
```

```python
# 1. 通过某个学生拓展表去获取学生信息
Student.objects.filter(stuinfo__s_addr='成都')
# 2. 通过学生表获取个人拓展表的信息
stu_in_stuinfo = StuInfo.objects.filter(s__s_name='刘备').first() # 得到学生对象
select_name = stu_in_stuinfo.s.s_name  # 通过关联属性查找另外一张表的属性
# 3. 获取python班下的所有学生的信息和拓展表的信息
Student.objects.filter(g__g_name='php01班')  # 通过关联属性查找
# 4. 获取python班下语文成绩大于80分的女学生
stu = Student.objects.filter(g__g_name='python')
stu.filter(s_yuwen__gt=80)
# 5. 获取python班下语文成绩超过数学成绩10分的男学生
stu.filter(s_yuwen__gte=F('s_shuxue')+10)
#6. 获取出生在80后的男学生，查看他们的班级
stu = Student.objects.filter(s_birth__year__gte=2000).first()
stu.g.g_name
```

<<<<<<< Updated upstream
[TOC]
=======

>>>>>>> Stashed changes

