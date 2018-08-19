### django基础教程简记

---

[Django学习文档博客](http://www.cnblogs.com/Simon-xm/tag/Django/)

```python
class Meta:   # 指定模型的复数形式,不指定会在admin中自动加s
   verbose_name_plural = 'categories'
      
verbose_name  指定一个用户可读的名字

# admin.py
prepopulated_fields = {'slug': ('aa', 'bb')}
该方法会自动合并aa 和 bb 为aa-bb形式填写到该字段输入框中
```

官方文档[prepopulated_fields](https://docs.djangoproject.com/en/dev/ref/contrib/admin/#django.contrib.admin.ModelAdmin.prepopulated_fields)





#### ForeignKey.on_delete

- 当一个ForeignKey 引用的对象被删除时，Django 默认模拟SQL 的ON DELETE CASCADE 的约束行为，并且删除包含该ForeignKey的对象。这种行为可以通过设置on_delete 参数来改变。例如，如果你有一个可以为空的ForeignKey，在其引用的对象被删除的时你想把这个ForeignKey 设置为空：

- user = models.ForeignKey(User, blank=True, null=True, on_delete=models.SET_NULL)

- on_delete 在django.db.models中可以找到的值有：

  - CASCADE
    - 级联删除；默认值。
  - PROTECT
    - 抛出ProtectedError 以阻止被引用对象的删除，它是django.db.IntegrityError 的一个子类。
  - SET_NULL
    - 把ForeignKey 设置为null； null 参数为True 时才可以这样做。
  - SET_DEFAULT
    - ForeignKey 值设置成它的默认值；此时必须设置ForeignKey 的default 参数。
  - DO_NOTHING
    - Take no action. 如果你的数据库后端强制引用完整性，它将引发一个IntegrityError ，除非你手动添加一个ON DELETE 约束给数据库自动（可能要用到初始化的SQL）。

- SET()

  - 设置ForeignKey 为传递给SET() 的值，如果传递的是一个可调用对象，则为调用后的结果。在大部分情形下，传递一个可调用对象用于避免models.py 在导入时执行查询：

  ```python
  from django.conf import settings
  
  from django.contrib.auth import get_user_model
  
  from django.db import models
  
  def get_sentinel_user():
  
      return get_user_model().objects.get_or_create(username='deleted')[0]
  
  class MyModel(models.Model):
  
      user = models.ForeignKey(settings.AUTH_USER_MODEL,
  
                               on_delete=models.SET(get_sentinel_user))
  
  ```

  
  



##### 编写数据库填充脚本 p-53



![](QQ截图20180526171717.png)



#### [版本的中间键MIDDLEWARE](https://blog.csdn.net/xiongjiezk/article/details/53220302)

- 1.X -  MIDDLEWARE_CLASS
- 2.X - MIDDLEWARE



#### 伺服媒体文件

settings.py 中增加

```python
MEDIA_DIR = os.path.join(BASE_DIR, 'media')

MEDIA_ROOT = MEDIA_DIR

MEDIA_URL = '/media/'

TEMPLATE中的'context_process'
增加  -->  'django.template.context_processors.media'
```

调整URL

- 在项目的urls.py 文件中增加模块

  ```python
  from django.conf import settings
  from django.conf.urls.static import static
  
  urlpatterns = [
      
  ] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
  ```

**在网页中伺服媒体文件**

- 把媒体文件放到项目的media目录中, 
- 在模板页面中使用{{ MEDIA_URL }}上下文变量引用媒体文件
  - 例如  -- `<img src='{{MEDIA_URL}} cat.jpg'/>`



#### 建立模型-->>models.py



- ForeignKey  建立一对多关系
- OneToOneField  建立一对一关系
- ManyToManyField   建立多对多关系



模型中的**`__str__()`**

- 如果不使用`__str__()`, 那么在调试时, print打印出的对象将显示为`<Category: Category object>`,这将无法对我们的调试有所帮助, 添加`__str__()`后, 打印'Python' 分类时将显示`<Category: Python>`这个方法也可以在管理员界面用到,应为Django的后台显示对象为字符串表示形式



#### Django模型和Shell

- `python manage.py shell`
- 启动模型交互环境



#### 管理员界面

- 



#### [url-中的正则匹配问题](https://www.cnblogs.com/Simon-xm/p/3890759.html)