Django配置Mongodb



本文推荐用virtualenv创建虚拟环境来管理开发环境，因为这样你可以让自己的开发环境变的干净，整洁。virtualenv的官方文档[virtualenv](https://virtualenv.pypa.io/en/stable/) 
 关于virtualenv简单的使用请看过来。 
 **windows system** 
 打开doc输入窗口：

```
1.安装virtualenv
pip install virtualenv
virtualenv --version         #查看virtualenv版本
2.在环境目录安装开发环境
3.cd 工作目录
4.virtualenv  --python=python2.7  [VENV name] #
5.激活虚拟环境
[VENV name]\Scripts\bin\activate
6.安装其他的依赖包
pip install django==1.6.2
pip install pymongo==2.8
pip install mongoengine==0.9.0
pip install django-mongonaut12345678910111213
```

 **Linux system** 
 打开Linux终端

```shell
1.安装virtualenv
pip install virtualenv
virtualenv --version         #查看virtualenv版本
2.在环境目录安装开发环境
3.cd 工作目录
4.virtualenv  --python=python2.7  [VENV name] #
5.激活虚拟环境
source [VENV name]\bin\activate
6.安装其他的依赖包
pip install django==1.6.2
pip install pymongo==2.8
pip install mongoengine==0.9.0
pip install django-mongonaut12345678910111213
```

#### **3.创建django项目**

此步骤，大家可以参考django的用法，后面我会根据此文档创建项目并放到github上，感兴趣的可以关注。

#### **4.在项目中修改配置**

settings中修改配置

```python
在settings中添加以下标记字段
    INSTALLED_APPS = (   
        'mongonaut',          #添加此字段
        'django.contrib.admin', 
        'django.contrib.auth'，    
        'mongoengine.django.mongo_auth', #添加此字段
        'django.contrib.contenttypes',  
        'django.contrib.sessions',  
        'django.contrib.messages',  
        'django.contrib.staticfiles',  
        'blogapp',  
)
### 添加以下字段###
    from mongoengine import * 
    DBNAME = 'blog’
    AUTHENTICATION_BACKENDS = (  
        'mongoengine.django.auth.MongoEngineBackend',  
    )  
    SESSION_ENGINE = 'mongoengine.django.sessions'  
    ### 数据库做以下配置 ###
    DATABASES = { 
        'default': { 
            'ENGINE':'django.db.backends.sqlite3', 
            'NAME': os.path.join(BASE_DIR, 'db.sqlite3'), 
            'USER': '', 
            'PASSWORD': '', 
            'HOST': '', 
            'PORT': '', 
        } 
    }

    AUTH_USER_MODE= 'mongo_auth.MongoUser'  
123456789101112131415161718192021222324252627282930313233
```

urls中修改配置

```python
### urls.py 中需要添加的字段 ###

    from django.conf.urls import patterns, include, url 
    import mongonaut 
    from mongonaut import urls 
    from django.contrib import admin 
    from blogapp.views import * 
    admin.autodiscover() 

    urlpatterns = patterns('', 
        # Example 
        url(r'^mongonaut/', include('mongonaut.urls')),
        url(r'^admin/', include(admin.site.urls)),   12345678910111213
```

models中配置

```python
###models.py需要引入的模块###
    from django.db import models 
    from mongoengine import *  
    from blog.settings import DBNAME 
    connect(DBNAME)  
    class Employee(Document): 
        name = StringField(max_length=50)
        age = IntField(required=False)  12345678
```

新建mongoadmin.py的文件

```python
###新建一个mongoadmin.py的文件###
    # import the MongoAdmin base class
    from mongonaut.sites import MongoAdmin
    ###import your custom models###
    from blogapp.models import Employee
    # instantiate the MongoAdmin class
    # Then attach the mongoadmin to your model
    Employee.mongoadmin = MongoAdmin()12345678
```

##### 接下来运行syncdb 创建管理员

##### 然后先用admin登录

*一定要记得先用admin账号登陆，然后才能在后面用mongonaut登陆，否则登陆不了后台。*

##### 在用mongonaut登录即可