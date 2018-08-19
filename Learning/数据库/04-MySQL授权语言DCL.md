

面试题   

关系型数据库中数据完整性指的是什么?

1. 实体完整性: 每条记录都是独一无二的没有重复的  (主键/唯一约束/唯一索引)
2. 参照完整性: 表中的数据要参照其他表已有的数据(外键)
3. 域完整性: 数据是有效的 (满足建表的约束 - 数据类型/非空约束/默认值约束/检查约束)

### 三范式

##### 表的设计原则 : 范式理论(1NF/2NF/3FN/BCNF)

- 范式级别表示表的规范程度, 范式级别越高表示表的规范程度越高
- 范式级别越高在插入 /删除/跟新数据时发生的问题越少
- 表中的数据冗余程度也就越低
- 实际中范式级别越高会更加占用时间越多
- 实际开发中往往会降低范式级别来提升数据查询的性能
- 1NF  - 列的属性值不能再拆分   (每个列只能有一个属性)
- 2NF - 所有的列要完全依赖于主键  (除了 主键列 之外的列完全依赖于主键)
  - 场景: 不同学院的学生可能有相同的学号, 一张表中有学生的id 和属性  还有学院的id 和属性
  - 学生表(stuid, sname, ssex, did, dname, dtel)   
  - 主键(stuid, did)       添加一个酱油字段 (id) 作为主键可以避免使用复合主键
- 3NF - 消除传递依赖
  - 场景: 整个学校学生的学号是唯一的, 可能会出现学生同时在两个学院读书的情况
  - 学生表: (stuid, sname, ssex, did, dname, dtel)
  - 主键 (stuid)    重复出现的数据, 不满足第三范式 
  - 将表拆分成两张表,可以消除传递依赖

  

#### DCL

- 数据控制语言  授予权限和召回权限
- grant 授权  grant all on xxx to xxx
- revoke 召回权限  revoke all on xxx from xxx

```mysql

-- 创建用户并指定登录口令
create user hellokitty identified by '123123';
-- 授予权限和召回权限
grant all on school.tb_student to hellokitty;
revoke all on school.tb_student from hellokitty;
grant select on school.tb_student to hellokitty;
grant all on school.* to hellokitty;
grant all on *.* to 'hellokitty'@'%';
revoke all on *.* from hellokitty;
-- 删除用户
drop user hellokitty;
```

