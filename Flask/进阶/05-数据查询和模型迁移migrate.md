05-模型的一对多查询和数据迁移



#### 1. 一对多查询

模型

```python
from flask_sqlalchemy import SQLAlchemy

db = SQLAlchemy()

class Student(db.Model): # Many
    __tablename__ = 'tb_student'
    s_id = ... # 主键
    ...
    s_grade = db.Column(db.Interger, db.ForeignKey('tb_grade.g_id'))
    
class Grade(db.Model):  # one
    __tablename__ = 'tb_grade'
    g_id = # 主键
	...
    students = db.relationship('Student', backref='stu', lazy=True)
```



#### 查询

```python

# 通过班级查学生,  g_id=2
grade = Grade.query.filter(Grade.g_id==2)
students_list = grade.studnets.all()

# 通过学生查询班级 s_id = 1001
student = Studnent.query.filter(Student.s_id==1001)
grade = student.stu    # relationship 中的 backref=stu
```

> 表的外键由db.ForeignKey指定，传入的参数是表的字段。db.relations它声明的属性不作为表字段，第一个参数是关联类的名字，backref是一个反向身份的代理,相当于在Student类中添加了stu的属性。 

一对多, 多对多都是使用相同的原理来进行查询



### 2. 数据库迁移

在django中继承了makemigrations，可以通过migrate操作去更新数据库，修改我们定义的models，然后在将模型映射到数据库中。

在flask中也有migrate操作，它能跟踪模型的变化，并将变化映射到数据库中

#### 安装migrate

```
pip install flask-migrate
```

#### 配置使用migrate

初始化，使用app和db进行migrate对象的初始化

```
from flask_migrate import Migrate

#绑定app和数据库
Migrate(app=app, db=db)
```

##### 2.2.2 安装了flask-script的话，可以在Manager()对象上添加迁移指令

```
from flask_migrate import Migrate, MigrateCommand

app = Flask(__name__)

manage = Manager(app=app)

manage.add_command('db', MigrateCommand)
```

操作：

```
python manage.py db init  初始化出migrations的文件，只调用一次

python manage.py db migrate  生成迁移文件

python manage.py db upgrade 执行迁移文件中的升级

python manage.py db downgrade 执行迁移文件中的降级

python manage.py db --help 帮助文档
```



