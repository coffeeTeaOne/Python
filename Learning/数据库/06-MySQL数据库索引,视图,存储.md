

#### 索引

- 索引会加速查询, 但是会使增删改变慢 , 每次都要更新索引
- 根据需求建索引, 经常查询的属性
- 索引相当于是一个目录, 可以加速查询提升查询效率
- 索引是典型的空间换时间的技术,增加了索引是的查询更加快捷查询到想要的内容

```mysql
-- 创建索引   自动给 主键 和外键 加索引 
create index IDxStudent on TbEmp(ename);
--           给索引命名        那张表的那个列
-- 查看索引
show index from tbemp;
-- 删除
drop index 索引名;
-- 唯一索引
CREATE UNIQUE INDEX indexName ON
mytable(username(length)) 
```

[菜鸟教程索引详细讲解](http://www.runoob.com/mysql/mysql-index.html)

#### 视图

`select or replace view... as.. `

- 动态生成的衍生表
- 相当于将表中的某些列映射出来到一个视图中, 方便以后查看, 后续查看时只需要查看已经创建的视图
- 在授权时, 可以将用户的授权放在一个视图上, 给用户的查询权限进一步缩小到一张表中的某些列中
- 让不同的用户可以看到不同的表中的不同的列的数据

```mysql
-- 创建视图
create or replace view 视图名 as 
select a, b from tb_remp;     查询的结果表 ;
-- 查看视图
select * from 视图名;
```

#### 存储过程

可以定义变量, 写循环和分支结构

- 函数和过程, 将重复的操作封装起来
- 函数可以产生返回值, 而过程是没有返回值的
- 函数和过程都是存储在数据库服务福安编译好的代码, 所以执行的效率比向数据库发出的SQL语句更快
- 如果希望简化调用并且鬼改善性能, 可以考虑使用存储过程
- 安全性提高, 网络截获请求数据时不会暴露查询语句, 导致数据库表结构暴露

```mysql
drop procedure if exists sp_dept_avg_sal
-- 创建存储过程  过程没有返回,通过 out 将结果带出
create procedure sp_dept_avg_sal(
deptNo integer,   -- 传入部门编号
out avgSal float  -- 输出参数, 将 avgSal输出
)
begin;
select avg(sal) into avgSal from tbemp 
where dno=deptNo;
end;
-- 调用过程   mysql变量名  @ 开头  
call sp_dept_avg_sal(20 ,@avgSal);
select @avgSal;
```

#### 触发器

`tigger`

- 在增删改时会触发一些其他的事情
- 实际开发避免使用 ,会导致SQL性能降低