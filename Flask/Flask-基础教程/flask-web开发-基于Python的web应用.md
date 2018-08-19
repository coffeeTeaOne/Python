>作者: 张明禄
>
>说明: 本文主要是学习\<Flask Web开发：基于Python的Web应用开发实战>一书而写的笔记, 内容有很多的截图都是来自此书, 想要学习更多请下载官方正版进行阅读



flask web开发, 基于Python的web应用

---



### 请求钩子

在==处理请求之前==或者在==请求之后==执行的代码

-   请求钩子使用装饰器实现, Flask支持下面四种
-   before_first_request ; 注册一个函数, 在处理第一个请求之前执行
-   before_request :   在每次请求之前运行
-   after_request : 如果程序运行没有异常抛出, 在每次请求之后执行
-   teardown_request : 即时有未处理的异常, 也会在每次请求之后执行

在请求钩子函数和视图之间共享数据一般使用的是上下文全局变量==g==

-   例如: before_request处理程序可以从数据库中加载已登录的用户, 并将其保存在==g.user==中, 随后调用视图函数时, 视图函数在调用==g.user==来获取用户



### 响应

自定义响应的状态码,

-   默认的请求成功状态码是200, 我们也可以在 ==return==中增加一个参数来自定义状态码

```python
@app.route('/')
def index():
    return 'OK', 400    在return中自定义状态码
```

还可以增加==第三个参数==, 这是一个由首部(header)组成的字典, 可以添加到HTTP响应中, 一般情况下不需要这么做



如果想传入多个参数, 但是不想以元组的形式得到数据,  可以定义 Response对象来进行返回, 返回的是==键值对==形式

```python
from flask import make_response

@app.route('/')
def index():
    response = make_response('<h1>OK</h1>')
    response.set_cookie('answer', '20')
    return response
```

重定向一般使用在表单中, 状态码默认的是 302

-   redirect()

还有一种特殊的响应式有==abort==函数生成的, 用于处理错误

-   例: 如果url中的动态参数 ID对饮的用户不存在, 就返回 状态码 404

```python
from flask import abort

@app.route('/uer/<id>')
def get_user(id):
    user = load_user(id)
    if not user:
        abort(404)
        return '%s' % user.username
```

baort不会吧控制权交还给调用他的函数, 而是抛出异常把控制权交给 web服务器



#### 使用Manager支持命令行选项

-   pip install  flask-script
-   启动可以自定义参数,-h 主机 ip   -p 端口, -d  debug模式
-   使用Manger来管理 app
-   runserver 启动服务/  shell 脚本  

```python
from flask_script import Manager

manage = Manager(app)

if __name__ == '__main__':
    manage.run()

```



### 模板

引入模板的作用是将业务逻辑和表现逻辑分离开, 即将数据和页面的HTML标签分离

-   存放的是表现逻辑,

##### Flask-Bootstrap

Jinja2提供了模板继承的机制, 并且 有  Flask-Bootstrap 提供了一个基本的页面模板, 所有的页面都可以继承 ==bootstrap/base.html== 页面, 通过添加bootstrap的 id 和 class 来给页面添加丰富的样式



![52888658882](assets/1528886588822.png)



#### Flask-Moment本地化日期和时间

`pip install flask-moment`

`from flask_moment import Moment`

`moment = Moment(app)`



```html
<p>The local date and time is {{ moment(current_time).format('LLL') }}.</p>
<p>That was {{ moment(current_time).fromNow(refresh=True) }}.</p>
```





#### 表单

Flask-WTF模块可以将处理 WEB 表单变为一个简单的过程

`pip install flask-wtf`



防止跨站请求伪造

设置`app.config['SECRET_key']='hard to guess string'`



### 数据库和模型

#### SQL还是NoSQL

-   SQl数据库擅长用于高效且紧凑的形式存储结构化数据, 这种数据库需要花费大量的精力保证数据库的一致性, 
-   NoSQL数据库放宽了这种对一致性的要求, 从而获得了性能上的优势
-   对中小型程序来说, SQL 和 NoSQL数据库都是很好的选择, 而且性能基本相当



配置数据库

-   SQLALCHEMY_DATANASE_URI  配置数据库 指定 驱动 数据库种类 主机,用户 密码 数据库名称
-   SQLALCHEMYZ_COMMIT_ON_TEARDOWN  设置为 True时, 每次请求结束后都会自动提交数据库中的变动





#### 定义模型



Flask-SQLAlchemy 创建的数据库实例模型提供了一个基类以及一系列的辅助类和辅助函数, 用于定义模型的结构

-   `__tablename__`指定在数据库中 的表的名称, 默认是 类名的小写
-   `db.Column()` 指定字段的属性



| 类型名       | Python类型         | 说明             |
| ------------ | ------------------ | ---------------- |
| Integer      | int                |                  |
| SmallInteger | int                |                  |
| BigInteger   | int 或者 long      |                  |
| Float        | flaot              |                  |
| Numcric      | decimal.Decimal    | 浮点数           |
| String       | str                |                  |
| Text         | str                | 优化后的长字符串 |
| Unicode      | unicode            |                  |
| UnicodeText  |                    | 长Unicode字符串  |
| Boolean      | bool               |                  |
| Date         | datetime.data      |                  |
| Time         | datetime.time      |                  |
| Datetime     | datetime.datetime  |                  |
| Interval     | datatime.timedelta |                  |
| Enum         | str                |                  |
| PickleType   | 任何python对象     |                  |
| largeBinary  | str                |                  |



关键字

-   primary_key     True表示当前字段为主键
-   autoincrement    自增, 一般用于主键
-   unique     True 表示当前字段是唯一的不可重复
-   index  True  表示将当前字段创建为索引, 提升查询效率
-   nullable   True  允许使用空值
-   default  设置默认值



\__repr()__  返回一个可读性的字符串, 提升在调试时的可读性



#### one TO many 关系

```python
class Role(db.Model): # one
    # ...
    user = db.relationship('User', backref='role')
    
class User(db.Model):  # many
    # ...
    role_id = db.Column(db.Integer, db.ForeignKey('tb_role.id'))
    
```



| 选项名       | 说明                                                         |
| ------------ | ------------------------------------------------------------ |
| backref      | 在关系的另外一个模型中添加反向引用, 一般在  ==one==的一方    |
| primaryjoin  | 在指定两个模型之间使用的联结条件, 在模棱两可的关系中需要指定 |
| lazy         | 指定加载相关记录, 可选值有 select(首次加载时使用),immediate(源对象加载后就立即加载), joined(加载记录,但不适用联结), subquery(立即加载, 但使用子查询), noload(永不加载), dynamic(不加载记录, 但提供记录查询) |
| uselist      | False 表示不适用列表, 而使用标量                             |
| order_by     | 指定排序字段                                                 |
| secondary    | 指定多对多关系中的表的名字                                   |
| econdaryjoin | SQLAlchemy无法自行决定时, 指定多对多关系的二级联结条件       |



`db.session.add() ` 添加一个

`db.session.add(['','',''])`添加多个, 都可以放置在list中



删除行`db.session.delete()  db.session.commit()`



#### 常用的SQLAlchemy过滤器

| 过滤器                    | 说明             |
| ------------------------- | ---------------- |
| filter(Modelname.属性=值) | 返回一个新的查询 |
| filter_by(属性=值)        |                  |
| limit()                   | 指定数量查询     |
| offset()                  | 偏移查询         |
| order_by()                | 排序             |
| group_by()                | 分组             |

#### 查询函数

| 方法           | 说明                                |
| -------------- | ----------------------------------- |
| all()          | 以列表形式返回查询结果              |
| first()        | 获取查询结果的第一个,  列表除外     |
| first_or_404() | 返回第一个或者 404错误响应          |
| get()          | 返回指定==主键==的行, 默认但会 None |
| get_or_404()   |                                     |
| count()        | 返回查询的数量                      |
| pagiante()     | 返回一个paginate对象, 用于分页      |



### 电子邮件支持  Flask-mail

`pip install flask-mail`





