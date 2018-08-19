Dgango-05



```python
widget = forms.HiddenInput()   # 表单小组件, 隐藏字段
initial = {}  添加表单初始数据   # 41 行
调用 is_valid 时, 写的额外的验证时也会执行
redirect('url')   重定向, 重新加载
```



#### Form 和 ModelForm

- 第 16 行 和 第 45 行相同
- ModelForm  
  - exclude=()  排除字段
  - help_text=''  放在 `<span>`中

```python
# Form
class CarRecordForm(forms.Form):

    carno = forms.CharField(max_length=7 ,label='车牌号',
                            error_messages={})
    case = forms.CharField(max_length=50, label='违章原因')
    punish = forms.CharField(max_length=50, required=False, 
                             label='处罚方式') #required 属性False可以不填
    
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
    return render(request, 'add.html',
                  context={'f': f, 'errors': errors})



# ModelForm
class CarRecordForm(forms.ModelForm):

    carno = forms.CharField(max_length=7 ,label='车牌号', error_messages={})
    case = forms.CharField(max_length=50, label='违章原因')
    punish = forms.CharField(max_length=50, required=False,
                             label='处罚方式') #required 属性False可以不填
    
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



![](get&post.png)



![](HTTP请求.png)



**HTTP无状态协议**

- 不能保存之前的结果和做的事情





#### 用户跟踪和识别

1. 隐藏域, 隐式表单域, 在提交表单是添加额外的隐藏字段, 表明用户身份
2. url重写, 在 `?` 后面  添加额外的键值对, 向服务器提供身份标识, 不能超过2048字节
3. **Cookie** , 放在请求头里面,  服务器用来识别用户身份的标识, 禁用后服务器无法识别,



#### Cookie

- 主流的验证方式, 在浏览器中的临时文件保存用户的身份标识, 偏好设置, 只能保存4kb内容, 一般只会保存标识性信息
  - 会话(服务器) session (浏览器保存文件, cookie存储有限, 其他保存在session中)
- 浏览器 - > 开发者工具 - > Application中 查看
- sessionid 会话Id  用户的身份标识, django中有  django_session表中有session
  - exoire_date失效日期
- 在每次请求服务器时都会在HTTP协议的请求头中都会携带本网站的Cookie设置
- 因为HTTP本省是无状态的, 所以需要使用Cookie/隐藏域/Url重写来实现用户跟踪

##### 自建项目创建Cookie

- 记录上次访问时间



- cookie是一个字典



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

![](cookie.png)



#### 对数据进行处理

![](清理数据.png)



#### forms.ModelForm额外添加条件验证数据

- 

```python
class CarRecordForm(forms.ModelForm):

    carno = forms.CharField(max_length=7 ,label='车牌号', error_messages={})
    case = forms.CharField(max_length=50, label='违章原因')
    punish = forms.CharField(max_length=50, required=False, label='处罚方式') #required 属性False可以不填
    
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
        model = Car
        fields = ('carno', 'case', 'punish')
```



### session

- 服务器端, 与cookie对应(浏览器端)

#### 拦截过滤器模式

- MIDDLEWEAR    -  中间键
  - 拦截过滤器, 拦截请求和响应, 附加操作, 在这里统一处理
  - HTTPresponse,   Httprequest

- 将处理请求代码剥离出来, 最为中间键





#### session应用

- 通过request的session属性可以获取到session
- session 相当于是服务器端用来保存用户数据的一个字典
- session 利用了Cookie保存sessionid 通过sessionid就可以获取用户会话的
- 如果在浏览器中清除了Cookie那么也就清除了sessionid
- 再次访问服务器时会重新分配新的sessionid 之前的用户数据将失效
- 默认情况下Django的session被设定为持久化而不是浏览器续存期会话
- 通过`SESSION_EXPIRE_AT_BROWSER_CLOSE` 和`SESSION_COOKIE_AGE`可以修改默认的模式和参数, 改变session的存在时间
- Django中的session是进行了持久化的处理的, 因此需要设定session 的序列化方式, 
- 从1.6版开始 Django默认的序列化方式是`JsonSerializer`
- 可以通过``SESSION_SERIALIZER`来设定其他的序列化器
  - 例如通过`PickleSerializer`
  - `SESSION_SERIALIZER = 'django.contrib.sessions.serializers.PickleSerializer'`



views.py

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



