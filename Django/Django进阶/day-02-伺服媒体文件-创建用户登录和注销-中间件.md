

Day02 伺服媒体文件, 创建用户登录和注销, 中间键

[TOC]

---



### 1. 表单提交和伺服媒体文件

**表单提交中的==自动返回原路径==**

- 即==POST==请求会从==GET==请求的路径发出

上传文件, 伺服媒体文件

- 创建==media==目录
- `pip install pillow`
- ==setting.py==增加配置
- ==url== 增加路径
- ==`s_img = request.FILES.get('s_img')`==

模板页面

```html
# 页面上传图片
设置表单提交模式 增加  enctype='multipart/form-data'  <!--设置编码格式-->
<input name='xx_img' type='file'>

#页面显示图片
<img src="/media/{{ student.s_img }}" alt=""/> 

```

==models 和 views urls==

```python
# models.py   会自动创建文件目录 upload 保存上传的文件,数据库中保存的是文件地址
s_img = models.ImageField(upload_to='upload', null=True) 

# urls 添加
from your_project.contrib.staticfiles.urls import static
from your_project.settings import MEDIA_ROOT, MEDIA_URL

urlpatterns += static(MEDIA_URL, document_root=MEDIA_ROOT)

# settings.py 增加配置文件路径
# 上传文件目录
MEDIA_URL  = '/media/'
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')

# 后台获取图片
s_img = request.FILES.get('s_img')
```



###2. 用户创建和验证

#### ==django自带的  auth  user 模块==

all()  判断是否为空, 空位False

要对views视图和url 进行编辑

- 可以在页面上对user进行验证
- user.is_

==views==

```python
from django.contrib.auth.models import User
from django.contrib import auth
from django.http import HttpresponseRedirect
from django.core.urlresolves import revrese

# 注册
def register(request):
    if request.method == 'GET':
        msg = None
        return render(request, 'register.html', {'msg':msg})
    if request.method == "POST":
        username = request.POST.get('username').strip()
        password = request.POST.get('password')
        re_pwd = request.POST.get('re_pwd')
        if not all([username, password, re_pwd]):
            msg = '用户名或密码不能为空'
            return render(request, 'register.html', {'msg':msg})
        elif: password != re_pwd:
                msg = '数入密码不一致'
                return render(request, 'register.html', {'msg':msg})
        # 满足所有条件后
        User.objects.create_user(username=username, password=password)
        return HttpResponseRedirect(reverse('app:login'))
    
# 登录
def login(request):
    if request.method == "GET":
        return render(request, 'login.html')
    if request.method == 'POST':
        username = request.POST.get('username')
        password = request.POST.get('password')
        # 验证身份, 用django自带的 auth.authenticate
        user = auth.authenticate(username=username, password=password)
        if user:
            auth.login(request, user) # 将user在数据库登录保存
            return HttpResponseRedirect(resverse('app:index'))
        else:
            msg = '输入错误'
            return render(request, 'login.html')
        
# 注销用户
def logout(request):
    if request.method == 'GET':
        auth.logout(request)
        return HttpResponseRedirect(reverse('app:login'))

```

==url==

- 对每个Url都需要进行包装, 用`login_requeired`

```python
from django.conf.urls import url
from app import views
from django.contrib.auth.decorators import login_requeired

urlpatterns = [
    url(r'index/', login_required(views.index), name='index')
]
```



#### ==自定义数据库来操作用户登录==

- 将user的登录这些行为放在一个app中
- 创建模型  
  - User.objects.create() 创建不用save(),   
  - User(username=username) 要save(), 才能保存在数据库中
- ==models==

```python
from django.db import models

class UserModel(models.Model):
    username = models.CharField(max_length=10)
    password = models.Charfield(max_length=100)
    ticket = models.CharField(max_length=30)  # 验证身份的标志
    create_time = models.DateTimeField(auto_now_add=True) # 不会改变
    login_time = models.DateTimeField(auto_now=True) # 每次操作都会改变
    
    class Meta:
        db_table = 'user'
```

- ==views==

```python
from user.models import UserModel

# 注册
def register(request):
    if request.method == "GET":
        return render(request, 'register.html')
    if request.method == 'POST':
        username == request.POST.get('username')
        password = request.POST.get('password')
        re_pwd = request.POST.get('re_pwd')
        if not all([username, password, re_pwd]):
            msg = '不能为空'
            return render(request, 'register.html')
        if password != re_pwd:
            msg = '密码不一致'
            return render(request, 'register.html')
        UserModel.objects.create(username=username, password=password)
        return HttpResponseRedirect(reverse('user:login'))
    
# 登录
def login(request):
    if request.method == 'GET':
        return render(request, 'login.html')
    if request.method == 'POST':
        username = request.POST.get('username')
        password = request.POST.get('password')
        user = UserModel.objects.filter(username=username, password=password)
        if user:
            # 生成随机的字符串作为验证的ticket
            t_str = 'rwqueoiruqwoieruoiq'
            ticket = ''
            for i in range(28):
                ticket += random.choice(t_str)
		   user.ticket = ticket
            user.save()
            # 在cookie中绑定ticket
            response = HttpResponseRedirect(reverse('app:index'))
            response.set_cookie('ticket', ticket)
            return response

# 注销
def logout(request):
    if request.method == 'GET':
        reponse = HttpReponseRedirect(resverse('user:login'))
        reponse.delete_cookie('ticket')
        return response
```

##### 装饰器验证用户是否登录

```python
"""
    验证是否登录的装饰器
"""
def decoator(func):
    def wrapper(request): # 包装, 增加功能
        ticket = request.COOKIES.get('ticket')
        user = Users.objects.filter(ticket=ticket)
        if not user:
            return HttpResponseRedirect(reverse('user:login'))
        return func(request)
    return wrapper
```

### 3. 中间件

浏览器请求, 所有的请求都会先通过中间键来进行一次过滤,  满足条件才会进入下一步

- process_request 

views --> templates

- process_template_response

- process_views

- process_response    chuli

定义中间件

- 创建目录 utils - >新建 UserMiddleware.py   新建  \__init__.py  将其设置为项目内置的包
- settings.py 加载中间键 ==MIDDLEWARE==,
- 设置未登录时的跳转地址==LOGIN_URL = '/user/login/'==
- ``'utils.UserAuthMiddleware.UserAuthMiddle',``
- 会出现重定向次数过多的情况, 是因为中间件验证的登录页面也没有cookie, 所以一直在login页面重定向, 应该就在中间件中设置一下取消的重定向的路径, 在==request.path==中设置忽略的路径

```python
from django.http import HttpResponseRedirect
from django.utils.deprecation import MiddlewareMixin
from django.core.urlresolvers import reverse
from user.models import Users

class UserAuthMiddle(MiddlewareMixin):
    def process_request(self, request):
        # 验证cookie 中的ticket 验证不过, 跳转登录
        # 验证通过, request.user 当前登录用户信息
        # return None 或不 return   可以验证过期时间
        path = request.path
        # 忽略的请求s
        ignore_path = ['/user/login/', '/user/register/']
        if path in ignore_path:  # 避免重定向次数过多
            return None
        ticket = request.COOKIES.get('ticket')
        if not ticket:
            return HttpResponseRedirect(reverse('user:login'))
        user = Users.objects.filter(ticket=ticket)
        if not user:
            request.user = user
```



#### 重定向

`HttpResponseRedirect()`

- 参数可以是绝对路径也可以是相对路径
- 加`reverse`参数, 可以写命名空间

`redirect()`

- ​

`reverse()`

- 可以直接用views函数来进行重定向处理函数