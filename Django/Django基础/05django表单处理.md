day04





pip install django==1.11可以在终端中更改django的版本



#### 产品原型 

专业工具: 

- Axure RP 快速原型工具 - 线框图 - 高保真原型

业余工具

- Mockups  Mockplus



#### django 1.11

```python
#  url 
from django.conf.urls import url

urlpatterns = [
    url(r'^admin/$', admin.site.urls)   # 正则表达式
]

# 创建模型  models.py   继承models.Model
carno     不设置 主键会自动添加主键 
models.DateTimeField(db_column='happenday')  # 指定在数据库中的列名
models.BooleanField(default=False)
	class Meta:
        db_table = 'tb_ss_ss' 数据库中表名设置
  		oriding = ('xxx', )   设置排序

# 模型迁移
1. 配置数据库
2. 安装 pymysql --> pip install pymysql
3. 在 __init__.py  添加  import pymysql  pymysql.install_as_MySQLdb()
4. 创建数据库  指定默认编码  utf8  
5. settings 添加 app 
6. python manage.py makemigrations your_app     make migrate
7. 配置管理员界面   用户 密码设置
8. admin.py  注册模型 
	from search.models import Carrecord
    class carRecordAdmin(admin.ModelAdmin):
        
        lis_diaplay = ('', '')
        search_field = ('', )
    
    admin.site.register(carrecord)
9. 在templates 中创建静态模板页面   search.html

```



####令牌  {% csrf_token %}

cookie 用户跟踪   在浏览器里放置 cookie 做身份识别 ,首次登陆网站会生成

跨站身份伪造, 利用浏览器存储的cookie信息

在 DJango 的表单中放置令牌, 防止, 伪造

{% csrf_token%}   随机令牌, 令牌每次都是不同, 放置重复操作, 重放攻击

type='hidden'    隐藏域, 隐式表单域, 页面上无法看到该内容



#### request

- request.method    ==  'GET' / 'POST'
- META   请求头
- PATH  路径
- GET   查询参数   路径后面的参数  ?a=b&c=d  -->  {a:b, c:d}
- POST 表单参数   



Django  会自动按照 字典 - > 属性 -> 键 -> 方法 的顺序搜索

 

#### 调试



#### form

- action
- method
- input
  - type   search  submit
- datetime.strftime()  格式化时间日期



post 请求 添加  csrf_token   防止跨站身份伪造



JSON序列化编码器



内存困惑在线绘制内存图[pythontutor](http://pythontutor.com/visualize.html#mode=edit)

#### 深度复制

```python
import copy
copy.deepcopy()  全复制, 深拷贝, 深度复制克隆
copy.copy()  浅拷贝
```

pip show 包 查看是否需要安装的包



```python
# 序列化/串行化/腌咸菜: 把对象按照某种方式处理成字符或者字节的序列
# 反序列化/反串行化/解冻 : 把字符或者字节序列重新还原成对象
# python 序列化反序列化的工具  : json/  pickle / shelve / jsonpickle
pickle  # 序列化成字节
# django 序列化工具; serializers.serialze
from django.core.serializers import serialize
serialize('json', 对象)
```



#### Django表单处理



表单提交数据

- 终端
  - from car_no.views import CarRecorsform
  - f = CarRecordForm()
  - f.as_table()   as_p()P标签     as_ul() 列表

````python
f.is_valid()  查看输入是否有效
f.errors
````

```python
auto_now     第一次插入数据时记录时间, 不会修改
auto_now_add  最后登录时间, 每次修改都会改
```

#### 创建一个类 继承 forms.Form

#### 重写属性

```python
from django import forms

class CarRecordForm(forms.Form):
    carno = forms.CharField(max_length=7, label='车牌号码', error_messages={}) # label 显示在标签中的名字
    case = forms.CharField(max_length=50, label='违章原因')
    punish = forms.CharField(max_length=50, require=False, label='处罚方式')
    
def add(request):
    if request.method == 'GET':
        errors = []
        f = CarRecordForm
    else:
        f = CarRecordForm(request.POST)
        if f.is_valid():
            a = f.cleaned_data
            Car(**a).save()
            f.CarRecordForm()
        else:
            errors = f.errors.values()
    return render(request, 'add.html', comtext={'f': f, 'errors': errors})
```



#### views.py

```python
from json import dumps, JSONEncoder

from django.core.serializers.json import DjangoJSONEncoder
from django import forms
from django.http import HttpResponse, JsonResponse
from django.shortcuts import render

# Create your views here.
from car_no.models import Car


class CarRecordForm(forms.Form):
    carno = forms.CharField(max_length=7 ,label='车牌号', error_messages={})
    case = forms.CharField(max_length=50, label='违章原因')
    punish = forms.CharField(max_length=50, required=False, label='处罚方式') # 可以不填


def add(request):

    if request.method == 'GET':
        errors = []
        f = CarRecordForm()
    else:
        f = CarRecordForm(request.POST)
        if f.is_valid():
            a = f.cleaned_data # 获得输入的纯数据, 字典样式
            Car(**a).save()
            f = CarRecordForm()
        else:
            errors = f.errors.values()  # 所有的错误信息
    return render(request, 'add.html', context={'f': f, 'errors': errors})



class CarRecordEncoder(DjangoJSONEncoder):  # DjangoJSONEncoder
    # pass
    def default(self, o):
        print('=='*20, o.model)
        a = o.model.__dict__
        print('======>>>>>',a)
        del a['_state']    # 转化为字典
        # o.__dict__['cardate'] = o.happen_date
        return a

def ajax_search(request):
    if request.method == "GET":
        return render(request, 'ajax_search.html')
    else:
        carno = request.POST['carno']
        record_list = Car.objects.all()
        # 第一个参数是要转换成JSON格式(序列化)的对象
        #encoder 参数是指定自定义对象序列化的编码器 (JS
        # ONEncoder的子类型)
        # safe 的值如果为True 那么传入的第一个参数只能是字典, False 传入的不需要必须是字典, 但是要自定义序列化, 指定encoder
        #JsonResponse 如果传入的是字典, 就不需要其他的参数, 可以直接完成JSON格式的字符字符串的处理
        return JsonResponse(record_list,encoder=CarRecordEncoder ,safe=False)

        # return HttpResponse(json.dumps(record_list), content_type='application/json; charset=utf-8')


def search(request):

    if request.method == 'GET':
        ctx = {'show_result': False}
    else:
        carno = request.POST['carno']
        ctx = {'show_result': True,
            'record_list': list(Car.objects.filter(carno__contains=carno))} # contians 模糊查询

    return render(request, 'search.html', context=ctx)


def car(request):

    ctx={'car_list': Car.objects.all()}
    return render(request, 'car.html', context=ctx)


def query(request):
    ctx = {'car_list': Car.objects.all()}
    return render(request, 'query.html', context=ctx)


def index(request):
    ctx = {
        'query': '/car_no/query',
        'car': '/car_no/car'
    }
    return render(request, 'index.html', context=ctx)



```

