day04-REST框架API接口

---

[TOC]

#### 工具

- [ ] djangorestframework
  - pip install djangorestframework
- [ ] Postman  - 接口测试工具
- [ ] Ajax



#### 效果

- 前后分离
- 返回的是 render() 页面, 数据的加载在页面中利用的是Ajax技术来获取数据



GET : 获取所有数据   mixins.ListModelMixin

​         : 获取一条数据 - mixins.RetrieveModelMixin

POST : 创建   mixins.CreateModelMixin

PUT : 修改 - 全部属性 mixins.UpdateModelMixin

PATCH : 修改 - 部分属性  mixins.UpodateModelMixin

DELETE : 删除  mixins.DestroyModelMixin



#### 配置   分页和用户权限

```python
REST_TRAMEWORK = {
    'DEFAULT_PAGINATION_CLASS': 'rest_framework.pagination.PageNumberPagination',   
    'PAGE_SIZE': 5,  # 每页的条数
    'DEFAULT_AYTHENTICATION_CLASS': (),  # 取消rest自带的权限认证 
}
```

#### Ajax获取数据

```javascript
$.get(url, function () {});
$.post(url, function () {});
$.ajax({
    url: '',   # 请求地址
    type: '', # GET POST PATCH PUT DELETE
    data: {'name': name, 'sex': sex}  # 请求的参数, GET不需要参数, 修改和获取需要次参数
    dataType: 'json', # 传递数据的格式
    headers: {'X-CSRFToken': csrf}  # 需要传递csrf时添加
	success : function (msg) {},  # 成功执行的函数
	error: function (msg) {} #失败执行的函数
});
```



### 流程

1. `pip install djangorestframework`

2. setting.py文件

   - INSTALLED_APP 增加注册 APP `rest_framework`
   - 修改一下默认的配置, 可以增加分页  或者 取消默认的用户验证功能

   ```python
   # 修改一下设置  REST
   REST_FRAMEWORK = {
       # 'DEFAULT_PAGINATION_CLASS': 'rest_framework.pagination.PageNumberPagination',
       # 'PAGE_SIZE': 5,
       'DEFAULT_AUTHENTICATION_CLASS': (),  # 将 restframework默认的用户认证取消
   }
   ```


3. APP/urls.py文件增加注册rest_framework

   ```python
   from rest_framework.routers import SimpleRouter
   
   router = SimpleRouter() # 定义路由
   router.register(r'^api/student', views.api_student)  # 在路由中注册API, 每个API一个注册
   urlpatterns += router.urls   #添加, 使访问生效
   ```

4. 编写其序列化器,设置返回的参数格式和字段, serializer.py

   ```python
   from rest_framework import serializers
   from app.models import Student
   
   # 定义序列化   主要是继承器父类
   class StudentSerializer(serializers.ModelSerializer): 
       # 在创建时指定字段, 定制错误信息
       s_name = serializers.CharField(max_length=20,
                                      error_messages={}) 
       
       class Meta:
           model = Student
           fields = ('id', 's_name', 'g')  #设置要显示的书信
           
       def to_representation(self, instance): # 获得的对象, 进行数据的查找和组装
           data = super().to_representation
           data['g_name'] = instance.g.g_name
           return data
           
   ```
   错误信息定制![1527813251921](assets/1527813251921.png)

5. 写显示方法

   ```python
   from rest_framework import mixins, viewsets
   from app.serializer import StudentSerializer
   
   # 学生的API视图
   class api_student(mixins.ListModelMixin,
                    mixins.CreateModelMixin,
                    mixins.RestrieveModelMixin,
                    mixins.UpdateModelMixin,
                    mixins.DestoryModelMixin,
                    viewsets.GenericViewSet):
       
       queryset = Setudent.objects.filter(s_delete=False)
       serializer_class = StudentSerializer
       
       def perform_destory(self, instance):
           instance.s_delete = True
           instance.save()
        
   
   def student(request):   # 只返回页面, 不带数据
       if request.method = 'GET':
           return render(request, 'student.html')
        
   ```

6. 页面Ajax

   ```javascript
   <script>
       $(document).ready(function () {
       $.get('/app/api/student/', function (msg) {
           # 的到的msg是一个列表,  里面装的是字典
           var tr_html_all = ''
           for (var i = 0; i < msg.length; i += 1) {
               var tr_html = '';
                       tr_html += '<tr>';
                       tr_html += '<td>' + msg[i].id + '<td>';
                       tr_html += '<td>' + msg[i].s_name + '<td>';
                       tr_html += '<td>' + msg[i].s_shuxue + '<td>';
                       tr_html += '<td>' + msg[i].g_name + '<td>';
                       tr_html += '<td><a href="javascript:;" onclick="delstu(' + msg[i].id +')">删除</a></td>';
                       tr_html += '<tr>';
                       tr_html_all += tr_html;
           };
           $('#tb_table').append(tr_html_all)
       });
   });    
   </script>
   ```




