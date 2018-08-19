02-连接Redis和MySQL

#### 简介

Flask默认并没有提供任何数据库操作的API

我们可以选择任何适合自己项目的数据库来使用

Flask中可以自己的选择数据，用原生语句实现功能，也可以选择ORM（SQLAlchemy，MongoEngine）

SQLAlchemy是一个很强大的关系型数据库框架，支持多种数据库后台。SQLAlchemy提供了高层ORM，也提供了使用数据库原生SQL的低层功能。

ORM：

```
将对对象的操作转换为原生SQL
优点
	易用性，可以有效减少重复SQL
	性能损耗少
	设计灵活，可以轻松实现复杂查询
	移植性好
```

针对于Flask的支持, 更多[官网地址](http://flask-sqlalchemy.pocoo.org/2.3/)



- Redis  主要用于会话的存储
- MySQL 主要用于数据的存储

#### 连接redis

```
  
  from falsk_session import Session
  
  # 设置 密钥  数据库  redis
      app.config['SECRET_KEY'] = 'secret_key'  # 添加自己定义的复杂的 secret_key
      app.config['SESSION_TYPE'] = 'redis'  # 选择数据库类型, 默认连接本地 redis
  
      # 连接指定的地址, 设置数据库的地址可以连接其他服务器的Redis
      app.config['SESSION_REDIS'] = redis.Redis(host='127.0.0.1',
                                                port=6379)
       # 添加 sessionid的前缀
      app.config['SESSION_KEY_PREFIX'] = 'flask'
      
      Session(app=app)  # 初始化
      # 也可以写为 Session().init_app(app=app)
```

 

#### 连接 MySQL

注意: 创建模型是需要 从 falsk-sqlalchemy 中 导入 SQLAlchemy开进行模型的创建

```
   # 配置数据库
      app.config['SQLALCHEMY_DATABASE_URI'] = 'mysql+pymysql://root:123456@localhost:3306/hello_flask'
      app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False
  
      db.init_app(app=app)
```

例：访问mysql数据库，驱动为pymysql，用户为root，密码为123456，数据库的地址为本地，端口为3306，数据库名称HelloFlask

设置如下： "mysql+pymysql://root:123456@localhost:3306/HelloFlask"

在初始化init.py文件中如下配置：

```
  app.config['SQLALCHEMY_TRAKE_MODIFICATIONS'] = False
  
  app.config['SQLALCHEMY_DATABASE_URI'] = "mysql+pymysql://root:123456@localhost:3306/HelloFlask"
```

 

 