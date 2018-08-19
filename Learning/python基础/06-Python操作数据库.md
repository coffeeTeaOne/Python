

ORM - Object Relation Mapping 对象关系映射

- 关系型数据库 - 关系模型
- Python 程序 - 对象模型
- 第三方框架 Alchemy 可以完成对象关系的双向转换,可以直接操作 数据库, 不用写 SQL 语句, 但是会降低性能

##### 关键字

*host / [port] / user / passwd / db / charset / [autocommit / cursorclass=pymysql.cursors.DictCursor]*

##### 操作方法

1. 调用的是pymysql的connect()/Cursor()方法
2. 创建连接  pymysql.connect()   输入

```python
import pymysql
conn = pymysql.connect(host='localhost', port=3306,user='root', passwd='123456', db='hrs',charset='utf8', autocommit=False)
```

1. 创建Cursor()
   - cursor.execute()  执行sql语句

```python
try:
	with conn.cursor() as cursor:
        result = cursor.execute('insert into tbdept values ()')
        conn.commit()
finally:
    conn.close()
        
```

- 在写sql语句时, 不可以用字符串格式化的方法来传参数,会被SQL注射攻击(SQL Injection) (x' or 1=1') 加一个恒成立的条件来跳过检查
- 使用其规定的占位符写法(占位符加元组)
- 也可以使用命名占位符  名称  + 字典

```python
# 安全占位符
result = cursor.execute('insert into tbdept values (%s, %s, %s)',(dno, dname, dloc))
# 命名占位 
result = cursor.execute('insert into tbdept values (%(no)s, %(name)s, %(dloc)s)', {'no': dno, 'name': dname, 'loc': dloc})
```

**用 select + fetchone() 对查找内容进行输出**

- 使用查询语句对数据库进行查询,返回的是一个元组
- 不可使用 select * from table 的方法对数据库进行查询,这样会先查询该数据库的属性然后在将属性和内容返回出来,变变为了两次查询,降低了查询的速度

```python
    try:
        with conn.cursor() as cursor:
            # 一般不适用 * 查询 ,
            cursor.execute('select dno, dname, dloc from tbdept')
            result = cursor.fetchone()
            while result:    # fetchone 每次抓去一条出来
                print(result[1])
                result = cursor.fetchone()
            conn.commit()
    finally:
        conn.close()
```

- 构建一个类, 将查到的结果(元组)出入, 通过类的构造方法对需要的属性进行查询
- 将 cursor 的类型改为字典 cursor  
  - cursorclass=pymysql.cursors.Dictcursor 

#### 常见的报出的错误

Cannot connect... host 错误  port 错误  服务器没有启动

Access denied ...  -user/ -passwd错误

Unknown database .... 数据库名字写错

MySQL syntax Error / unknow ... SQL 语句错误

默认情况root 只能本地连接,不能远程连接, 如果出现远程连接错误时, 将数据库的主机改为 % 