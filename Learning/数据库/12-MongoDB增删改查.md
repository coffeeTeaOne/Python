### mongodb



常用方法介绍

*   一般来说，我们创建集合用db.createCollection(name),如：db.createCollection("log"),创建一个名字为log的集合，没有任何的大小，数量限制，使用_id作为默认索引；
*   限制集合空间的大小：db.createCollection("log",{size:1024})或db.createCollection("log",{capped:true,size:1024}),创建一个名字为log集合，限制它的空间大小为1M，如果超过1M的大小，则会删除最早的记录；
*   限制集合的最大条数：db.createCollection("log",{max:1024})，创建一个名字为log集合，最大条数为1024条，超过1024再插入数据的话会删除最早的一条记录。这个不能使用capped:true，否则会报错；
*   即限制最大条数有限制使用空间大小：db.createCollection("log",{size:1024,max:1024})或db.createCollection("log",{capped:true,size:1024,max:1024})，限制集合最大使用空间为1M，最大条数为1024条。





#### 数据库操作

```sql

-- 查看所有的数据库
show dbs

-- 创建并切换到 spider 数据库
use spider

-- 删除当前数据库
db.dropDatabase()
```



#### 集合的创建、查看、删除

```sql

-- 创建并切换到 school 数据库
use school

-- 创建集合 colleges
db.createCollection('colleges')

-- 创建 students 集合
db.createCollection("students")

-- 查看 所有集合
show collections

-- 删除 students 集合
db.students.drop()

```



#### 文档操作 CRUD

```sql

-- 向students 集合中插入文档
db.students.insert({'s_id':'1', 's_name':'老李'})
                   
-- 插入方法 2
db.students.save({'s_id': '2', 's_name':'李白'})

-- 查看所有的文档
db.students.find()

-- 更新 s_id 为 1 的文档
db.students.update("s_id":'1', {"$set":{'s_name':'杜甫'，'age':'20'}}, upsert=true)

-- 查询所有的文档 pretty()
db.students.find().pretty()

                   
```



#### 筛选

>   注意:我们可以使用 find() 方法来查询指定字段的数据，将要返回的字段对应值设置为 1。但是除了 _id 你不能在一个对象中同时指定 0 和 1。否则同时指定0和1的话，会报错

```sql

-- 查询 s_id 大于 2 的文档 只显示 name 和 tel 字段
db.students.find({'s_id':{'$gt':'2'}}, {'s_id':'0', 'name','1', 'tel':'1'}).pretty()

-- 查询s_id大于2 的文档 显示 除 name 和 tel 外的字段
db.students.find({'s_id':{'$gt':'2'}}, {'s_id':'0','name':'0','tel':'0'})

-- 查询 s_id 大于2  的 只显示 s_id 和 tel  字段
db.students.find({"s_id",{'$gt':'2'}},{'s_id':'1', 'name':'1', '_id':'1'})

-- 查询学生文档 跳过第一条 只查 3 条
db.students.find().skip(1).limit(3).pretty()

-- 对查询结果今次那个排序（1 表示升序，-1表示降序）
db.students.find().sort({'s_id': '-1'})
```

