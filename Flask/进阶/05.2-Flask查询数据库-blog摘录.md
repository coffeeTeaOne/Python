Flask操作数据库

---



格式

```python
filter(model_name.属性名.运算符())

contains 包含
startswith 开始
endswith 结束

范围
in_
模糊查询
like('%search%')
__ge__() : 大于等于
__gt__() : 大于
__lt__() : 小于
__le__() : 小于等于
    
order_by
offset
paginator
```



**查询相关语句**：

```python
@stu.route('/selectstu/')
def select_stu():

    # 查询年龄小于16岁的学生的信息
    # 第一种查询的方式
    stus = Student.query.filter(Student.s_age < 16)
    # 第二种查询的方式
    # __lt__表示小于   __le__表示小于等于
    stus = Student.query.filter(Student.s_age.__lt__(16))

    # 查询年龄大于16岁的学生的信息
    # 第一种查询的方式
    stus = Student.query.filter(Student.s_age > 16)
    # 第二种查询的方式
    # __gt__表示大于   __ge__表示大于等于
    stus = Student.query.filter(Student.s_age.__ge__(16))

    # 查询年龄在16,17,20,23的学生的信息
    stus = Student.query.filter(Student.s_age.in_([16,17,20,23]))

    # 查询所有学生的信息
    # 第一种查询方法
    stus = Student.query.all()
    # 第二种查询方法
    sql = 'select * from student;'
    stus = db.session.execute(sql)

    # 按照id降序排列(注意：不能用：Student.query.all。order_by)
    # 因为：.all后  返回的是一个装有对象的列表，列表是不能排序的
    stus = Student.query.order_by('-s_id')

    # 按照id降序获取3个
    stus = Student.query.filter.order_by('-s_id').limit(3)

    # 获取年龄最大的学生的信息
    stus = Student.query.filter.order_by('-s_id').first()

    # offset(2)表示跳过2个数据   limit（3）表示截取3个信息
    stus = Student.query.filter.order_by('s_id').offset(2).limit(3)

    # 获取id等于24的学生
    # 第一种方法
    stus = Student.query.filter(Student.s_id==24)
    # 第二种方法(和Django 不同)
    stus = Student.query.get(24)


    # 查询多个条件（from sqlalchemy import and_,or_,not_ ）
    stus = Student.query.filter(Student.s_age==18,Student.s_name=='张三')
    # and_ 并且条件(和上一个的效果一样)
    stus = Student.query.filter(and_(Student.s_age==18,Student.s_name=='张三'))
    # or_或者条件
    stus = Student.query.filter(or_(Student.s_age==18,Student.s_name=='张三'))
    # not_ 非条件
return render_template('student_list.html', stus=stus)
```

**注意：**

*   stus = Student.query.filter(Student.s_age==18) 得到的结果是 **sqlalchemy.BaseQuery**
*   stus = Student.query.filter(Student.s_age==18).all() 得到的结果是 **装有对象的list(列表)**
*   stus = Student.query.get(1) 得到是一个对象，不是列表， ‘1’默认是s_id

**两张表一对多关联的查询：** 
学生表和班级表，一对多的关联

```python
from flask_sqlalchemy import SQLAlchemy
from datetime import datetime

db = SQLAlchemy()

class Student(db.Model):
    s_id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    s_name = db.Column(db.String(20), unique=True)
    s_age = db.Column(db.Integer, default=18)
    # 设置外键和班级表关联
    s_g = db.Column(db.Integer, db.ForeignKey('grade.g_id'), nullable=False)

    __tablename__ = 'student'

    def __init__(self, name, age):
        self.s_name = name
        self.s_age = age


class Grade(db.Model):
    g_id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    g_name = db.Column(db.String(10), unique=True)
    g_desc = db.Column(db.String(100), nullable=True)
    g_time = db.Column(db.Date, default=datetime.now)
    # 通过Grade和Student 的联系
    students = db.relationship('Student', backref='stu', lazy=True)

    __tablename__ = 'grade'

    def __init__(self, name, desc):
        self.g_name = name
        self.g_desc = desc
```

通过班级找到该班级下的学生：

```python
@grade.route('/selectstubygrade/<int:id>/')
def select_stu_by_grade(id):
    grade = Grade.query.get(id)
    # students 是在Grade模型类中定义的
    return render_template('student_grade.html', stus=stus,
                           grade=grade)
```

通过学生找到该学生在那个班级：

```python
@stu.route('/selectgratebystu/<int:id>')
def select_grade_by_stu(id):
    stu = Student.query.get(id)
    # .stu  是在Grade模型里定义的students里的backref = 'stu'
    grade = stu.stu
     return render_template('student_from_grade.html',grade=grade,stu=stu)123456
```

**两张表多对多的关系的查询：** 
学生表和课程表之间的多对多的关系查询： 
对应的stu.models：

```python
from flask_sqlalchemy import SQLALchemy

db= SQLALchemy()
# 多对多的中间表
sc = db.Table('sc',
db.Column('s_id',db.Integer,db.ForeiginKey('student.s_id'),primary_key =True),
db.Column('c_id',db.Integer,db.ForeignKey('course.c_id'),primary_key=True)
)

# 课程模型
class Course(db.Model):
    c_id = db.Column(db.Integer,primary_key=True,autoincrement=True)
    c_name = db.Column(db.String(10),unique=True)
    # Student表示学生的模型的类名，secondary=sc表示用的中间表(sc)，backref表示反向引用（通过学生(student)关联到课程（course））
    students = db.relationship('Student',secondary=sc,backref='cou')

    __tablename__ = 'course'

# 学生模型
class Student(db.Model):
    s_id = db.Column(db.Integer,primary_key=True,autoincrement=True)
    s_name = db.Column(db.String(20),unique=True)
    s_age = db.Column(db.Integer,default=18)

    __tablename__ = 'student'
```

向中间表中加入数据方法：

```python
@stu.route('/stucourse/', methods=['GET', 'POST'])
def stu_cou():
    if request.method == 'GET':
        stus = Student.query.all()
        cous = Course.query.all()
        return render_template('stu.cou.html', stus=stus, cous=cous)
    else:
        stu_id = request.form.get('student')
        course_id = request.form.get('course')
        # 原生的插入数据（第一种）
        # sql = 'insert into sc(s_id,c_id) values(%s,%s)' %(stu_id,course_id)
        # db.session.execute(sql)
        # db.session.commit()
        # 第二种写法
        stu = Student.query.get(stu_id)
        course = Course.query.get(course_id)
        # 在SC（中间表）中插入对应的学生id和课程id
        course.students.append(stu)

        db.session.add(course)
        db.session.commit()

        return 'ok'
```

在中间表中删除对应的数据方法： 
点击页面’删除’，get方法传入两个参数，一个是学生id，一个是课程id,

```python
@stu.route('/deletecourse/<int:s_id>/<int:c_id>/')
def delete_course(s_id, c_id):
    # 获取学生对象
    stu = Student.query.get(s_id)
    # 获取课程对象
    cou = Course.query.get(c_id)

    # 删除sc表中的对应的一条数据
    # cou.students.remove(stu)
    # 删除sc（中间表）中对应的数据
    stu.cou.remove(cou)
    db.session.commit()

    return redirect(url_for('stu.all_stu'))
```

通过学生查找学生选修的课程：

传入学生的id，后台接收id，

```python
@stu.route('/selectcoursebystu/<int:id>')
def select_course_by_stu(id):
    # 获取到学生对象
    stu = Student.query.get(id)
    # 通过学生对象找到课程对象
    cous = stu.cou
    return render_template('stucourse.html', cous=cous, stu=stu)
```