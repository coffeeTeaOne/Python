

- SQL(structured Query Langunage)
  - 关系型数据库的编程语言
  - DDL 数据定义语言 : creat 新建/ drop(删除) / alter (修改)
  - DML 数据操作语言 : insert 插入 delete 删除 update 更新
  - DQL 数据查询语言 : select
  - DCL 数据控制语言  : grant / revoke / begin / commit / rollback
- DDL操作  : creat 新建/ drop(删除) / alter (修改)
  - DDL 数据定义语言 基本操作指令 
    - 判断数据库存在 `drop datebase if exists school`
    - 创建数据库 `create datebase school default charser utf8`
    - 切换到已创建的 school 数据库 `use school`
    - 删除表 `drop table tb_student`
    - 创建表 `create table tb_student(stuid int not null comment '注释')` 多个并列用逗号隔开
    - 指定主键 `primary key (stuid)`
    - 修改表 `alter table tb_student add column saddr varchar(100);`
    - 删除表 `alter table tb_student drop column stel;`
- DML操作 : insert 插入 delete 删除 update 更新
  - 数据操作语言
    - 插入数据`insert into tb_student values ("写入要插入的内容与表的列数相同");`
    - `insert into tb_student ("要插入的属性,逗号隔开") values ("对应的属性值")`
    - 插入多个 , 圆括号里面装一组的属性,多组用逗号隔开
    - `insert into tb_student values ("插入数据1"), ("插入数据2");`



代码示例

```sql
-- 如果指定的数据库存在则删除该数据库
drop database if exists school;
-- 创建数据库  指定默认字符集 utf8
create database school default charset utf8;
-- 切换到school数据库
use school;
-- 删除表
drop table tb_student;
-- 关系型数据库是通过二维表来组织数据的
-- 创建学生表  加前缀区分
-- 主键(primary key)  能够标识唯一的一条记录的列 不能重复
-- comment 关键字后面跟的也是注释,单引号引起注释
create table tb_student
(
stuid int not null comment '学号', -- 添加约束  非空约束
sname varchar(10) not null comment '姓名', -- varchar 可变长度字符串 限定字符串最大长度
ssex bit default 1 comment '性别',  -- bit 只有 0 和 1 两种类型, default设定默认值 1
stel char(11) comment '电话号', -- char 定长字符串
sbirth date,  -- 日期型  datetime 时间日期型
primary key (stuid)  -- 将学号设置为主键
);

-- 修改表  添加属性  设置属性的参数
alter table tb_student add column saddr varchar(100);
-- 删除
alter table tb_student drop column stel;

-- DML  数据操作语言 insert 插入 delete 删除 update 更新
-- 插入学生记录  字符串用单引号  双引号也可以,不建议使用
insert into tb_student values(1001, '王大锤', 1, '1994-6-6', '成都力宝大厦');
insert into tb_student (stuid, sname) values(1002, '张');
insert into tb_student (stuid, sname, ssex) value(1003, '李飘飘', 0);

-- 一次加多个
insert into tb_student values
(1004, '张三丰', 1, '1990-3-4', '湖北武汉'),
(1005, '黄蓉', 0, '1990-3-4', '湖北武汉'),
(1006, '郭靖', 1, '1990-3-4', '湖北武汉');

```

