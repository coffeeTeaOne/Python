PostgreSQL常用命令

`$psql -U user_name -d database_name -h severhost`

#### 授权

```sql
-- create user 和 create role 的区别是 create user 指令创建的用户默认是有登陆权限的，而 create role 是没有的

-- 创建用户
create user username;
create user zhang;  -- 创建用户 zhang 默认有登陆权限

-- 创建角色并授予权限
create role role_name with permissions;
create role zhang with login; -- 创建用户 zhang 并赋予登陆权限

-- 赋给用户所有权限
grant all on tb_name to username； 

-- 撤销用户的权限
revoke permissions on tb_name from user_name;

-- INHERIT 权限： 该属性使组内的成员拥有组的所有权限
alter role test_user inherit;

-- 另外，在 postgresql 中 用户实际上就是 role，同时组也是role,包含其他role的role就是组
创建组：
create role user1;
grant user1 to role_admin;
grant user1 to role_test;
--切换 role
set role role_name;  -- 切换到 role_name 用户
rest role; -- 切换回最初的role

\c - username -- 切换用户
alter user username with password 'pd'; -- 修改用户的密码
```



#### 查看数据库

```sql

-- 查看所有数据库
mysql: show databases;
psql: \l  \list

-- 切换数据库
mysql: use dbname
psql: \c dbname

-- 查看所有的表
mysql: show tables
psql: \d

-- 查看所有的字段
mysql: show columns from table name
psql: \d tablename

-- 查看指定的表的基本情况
mysql: describe tablename
psql: \d+ tablename

-- 退出登陆
mysql: quit或\q
psql: \q

set search to schema -- 切换schema
explain sql   -- 解释或分析 sql  执行的过程 
```

#### 数据的增删改查

- 以 \ 开头 的都是在 shell 中运行的

```sql
-- 创建表
create table testcase (
	id integer,
    age integer,
    name text,
    primary key (id, name)
);
-- 自增 SERIAL, 默认从1 开始，会自己创建一张表进行自增字段的存储
create table testcase(
	id serial primary key,
    age integer,
    name text
);
-- 清空表
delete from tb_name; -- 每次删除一行，并在事物日志中删除一行
truncate table tb_name; -- 通过释放存储表数据所用的数据也来删除数据，并且只在事物日志中记录页释放
-- 删除表
drop table tb_name;

-- 查询数据
select * from testcase;
-- 插入数据
insert into testcase (id,task_class,age,name) 
values (4,2,'23','zhang');
insert into testcase (id, task_class) values (4, 4);

-- 修改字段
alter table testcase alter column age type int using age::int;
alter table testcase alter column name type varchar(64);

-- 添加字段
alter table testcase add column phone int8;
-- 更改字段名称
alter table testcase rename cloumn column_1 to column_2;

-- 插入数据
insert into testcase values (
81, 81, 18,'zng'
);
-- 如果表中字段有大写的字段，则需要对应的加上双引号。例：insert into test (no, "Name") values ('123', 'jihite');
-- 值用单引号引起来('')，不能用双引号（""）

-- 删除一行数据
delete from tb_name where column_name='zhang';

-- 更新数据
update tb_name set column_name='values' where column_name='';

-- 将两个查询的结果的 表进行做差
(select * from testcase where id > 3) 
except (select * from testcase where id=81);

-- 复制表
create table testcase_copy as select * from testcase;

-- 命令行运行
\o 'C:\Users\zhang\Desktop';

select * from testcase limit 2;

\x  打开或关闭扩展显示

-- 插入查询出的数据
insert into testcase_copy from testcase where id in (8, 10, 4);

-- 创建索引
create index idx_a on testcase_copy (name);

select * from testcase_copy order by idx_a asc;

-- 查看连接信息
select * from pg_stat_activity;

-- 数据库的备份shell
pg_dump -h IP -p port -d dbname -t tb_name -U postgres -W password -f file.sql

-- 恢复备份
psql -h IP -p -d -U -W -f 
```

