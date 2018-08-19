django小结02

---



> 表单提交,  request.method     cookie  session



#### 表单提交的令牌

`{% csrf_token%}`

- 随机令牌, 放置重复操作, 重放攻击, 只有每次都是不同是才表示不是恶意访问

- `type='hidden'` 隐藏域, 页面上不会显示`input`

- 通过模板对数据库中关联的表进行重写操作,验证有效性

  - 继承自`forms.Form` 和 `forms.ModelForm`

  **页面模板**

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>添加</title>
  </head>
  <body>
      <h2>添加违章记录</h2>
      <hr>
      <form action="/add" method="post">
          {% csrf_token %}
          <table>
              {{ f.as_table }}     <!--自动将f 中的属性调用生成表格-->
          </table>
          <input type="submit" value="添加">
      </form>
  </body>
  </html>
  ```

  **views.py控制器**

  ```python
  from django import forms
  
  class CarRecordForm(forms.Form):
      # 设置属性时必须和数据库中的设置是一致的, 不然无法提交到数据库中
      
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
  
  
  # ModelForm,  也可以添加额外的验证, clean_开头的方法写验证的条件
  class CarRecordForm(forms.ModelForm):
  
      carno = forms.CharField(max_length=7 ,label='车牌号', error_messages={})
      case = forms.CharField(max_length=50, label='违章原因')
      punish = forms.CharField(max_length=50, required=False,
                               label='处罚方式') #required 属性False可以不填
      
      def clean_carno(self):  # 验证数据, clean_开头,
                                # 加对应的字段名字 自动执行下面的验证规则
          
          _carno = self.cleaned_data['carno']
          if _carno in condition:
              #写验证条件, 验证通过
          	return self._carno
          else:
          	# 验证失败, 没通过
          	raise forms.ValidationError('错误信息')
      
      class Meta:
          model = CarRecord   # 指定模型对应的model, 在models.py文件中
          fields = ('carno', 'case', 'punish') # 与数据库模型对应的项目
          
          
  def add(request):
  
      if request.method == 'GET':
          errors = []
          f = CarRecordForm()# 添加初始化字符 initial = {}添加表单初始数据
      else:
          f = CarRecordForm(request.POST)
          if f.is_valid():
              f.save()
              return redirect('/search2')  # 重定向,跳转到search2
          else:
              errors = f.errors.values()  # 所有的错误信息
      return render(request, 'add.html', context={'f': f, 'errors': errors})
  ```



#### request 请求 method / POST /  GET

- `request.method == 'GET' / 'POST'`

- request里面有GET和POST属性

  - `request.POST`为表单提交的内容 

- 返回的内容, 返回JSON 字符串, (代码示例为==car_完整版==)

  ```python
  from django.http import HttpResponse, JsonResponse
  
  HttpResponse(json.dumps(your_data),content_type='application/json; charset=utf-8')
  
  # 也可以用 Jsonresponse, 但是要先自定义编码器, encoder
  
  return JsonResponse(your_data, encoder=YourEncoder, safe=False)
  
  class YourEncoder(DjangoJSONEncoder):  # 继承自DjangoJSONEncoder, 
                                         # 或者 JsonEncoder
      def defaulf(self, o):   #重写default方法
          a = o.model.__dict__
          del a['_satae']
          return a
      
  ```

- 返回渲染后的页面

  - 用`django.shortcuts`中的 `render`函数`render(request, 'your.html', context=ctx)`



#### cookie和session

Cookie - > 浏览器     session --> 服务器

- 用户跟踪的方式
  1. 隐藏域, 表单中的隐藏的额外的字段, 来表名用户的身份和喜好
  2. url重写,在url中添加额外的字段来识别用户的的身份, 不可以存放敏感数据, 大小不可超过2048字节
  3. **cookie**, 放在请求头中的, 服务器用来识别身份, 会在对应的session 中添加记录

##### Cookie

- 主流的验证方式, 一般只存放一些标志性的信息, 只能存放4Kb的信息, 其他的内容会存放在服务的会话(session)中, 通过Cookie可以调用Session

cookie 用户跟踪, 存放有sessionid

- HTTP是无状态协议, 所以需要这些信息来在用户每次登陆时进行用户的识别
- 每次的HTTP请求都会在请求头中携带用户的Cookie,来进行身份验证



示例, 对访问时间信息的cookie设置

```python
def search(request):
    current_time = datetime.datetime.now().ctime()
    last_vist_time = request.COOKIES.get('last_vist_time', 
                                         current_time)

    if request.method == 'GET':
        ctx = {'show_result': False, 'last_time': last_vist_time}
        response = render(request, 'search.html', ctx)
        response.set_cookie('last_vist_time', current_time,
                            max_age=1209600)
        return response
```



#### Session应用

- 通过request.session可以拿到==request==的session属性, 说明, request本身就是一个==object==
- session相当于服务器端用来存储用户数据的一个字典
- session 利用了Cookie保存的sessionid来对应器内容, 获取用户的会话信息
- 如果删除浏览器的Cookie那么其对应的sessionid也就会丢失, 再次访问会分配新的sessionid给用户, 即以前的用户记录将会丢失
- 通常情况下Django的session被设定的是持久化的, 而不是浏览器存续期的会话
- 修改session 的生命周期
  - `SESSION_EXPIRE_AT_BROWSER_CLOSE`和`SESSION_COOKIE_AGE`参数设定的该表



- 1.6版开始, Django 的默认序列化方式是`JsonSerializer`
- 修改Django的默认的序列化器, 改为`PickleSerializer`
  - settings.py - - >`SESSION_SERIALIZER='django.contrib.sessions.serializers.PickleSerializer'`

示例代码

```python
from datetime import datetime

from django.shortcuts import render, redirect

from cart.models import Goods


class CartItem(object):

    def __init__(self, no, goods, amount=1):
        self.no = no
        self.goods = goods
        self.amount = amount

    @property
    def total(self):

        return self.goods.price * self.amount


class ShoppingCart(object):

    def __init__(self):
        self.num = 1
        self.items = {}

    def add_item(self, item):
        if item.goods.id in self.items:
            self.items[item.goods.id].amount += item.amount
        else:
            self.items[item.goods.id] = item
            self.num += 1

    @property
    def total(self):
        val = 0
        for item in self.items.values():
            val += item.total
        return val


def index(request):
    goods_list = Goods.objects.all()
    return render(request, 'goods.html', {'goods_list': goods_list})


def add_to_cart(request, id):
    goods = Goods.objects.get(pk=id)
    cart = request.session.get('cart', ShoppingCart())
    item = CartItem(cart.num, goods)
    cart.add_item(item)
    request.session['cart'] = cart
    return redirect('/cart')


def show_cart(request):
    current_time = datetime.now().ctime()
    last_vist_time = request.COOKIES.get('last_vist_time', '第一次访问')


    cart = request.session.get('cart', None)
    cart_items = cart.items.values() if cart else []
    total = cart.total if cart else 0
    ctx = {
        'cart_items':cart_items,
        'total': total,
        'last_vist_time': last_vist_time
    }
    response = render(request, 'cart.html', ctx)
    response.set_cookie('last_vist_time', current_time)
    return response




```

