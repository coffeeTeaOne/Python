

#### 1.关键词

- `auto_increment`  自动增长
- `not null`不能为空
- `bit`  0 和 1
- `primary key () ` 主键
- `alter table  tb_one add column saddr varchar (100);` 增加新属性
- `alter table tb_one drop column stel;`删除属性
- `insert into tb_one (属性) values (属性对应的内容);`
- `delate from tb_one where stuid=1003;` 删除数据
- `update tb_one set sbirth='1994-3-3, saddr='成都' where stuid=1004 or stuid=1005 or stuid=1006;`更新数据
- `update tb_one set sbirth='1994-3-3, saddr='成都' where stuid=(1004, 1005, 1006);`集合用法更新数据
- 外键约束

```mysql
alter table tb_idcard add constraint fk_iscard_pid
foreign key (pid) references tb_person (persionid);
-- 唯一约束
alter table tb_idcard add constraint uk_idcard_pid
unique (pid);
-- 主键约束
alter table tb_idcard add constraint ky_idcard_pid
primary key (cardid);

-- 检查约束
alter table tb_record add constraint ck_record_returndate
check (returndate > borrowdate);
-- 一般设置在外键关联中
- on delate set null  -- 父表 update/delate记录时, 将子表匹配记录的列设置为空
- on delate cascade   -- 父表 update/delate记录时,同步 update/delate子表的记录
no action -- 如果字表中有匹配记录,则不允许对父表对应的键进行 update/delate
restrict  -- 和 no action 相同都是立即检查外键约束
-- 父表   外键约束引入的 属性 所在的表    字表   外键约束的表

```



#### 2.  建表时的关联1-1 1-N M-N

##### 1-1

* 外键约束  +  唯一性约束

##### 1-N

* 外键约束   --  用父表的列来 约束在子表上

##### M-N

* 外键约束作用在中间表上
* 中间表 + 外键约束 + 外键约束 



详解     示例代码  [coding代码地址](https://coding.net/u/zhangminglu/p/exampleCode/git/tree/master/db_demo?public=true)

##### 1-1

- 外键约束  +  唯一性约束

```mysql
alter table tb_husband add constraint fk_husband_wifeid 
foreign key (wifeid) references tb_wife (wifeid);

alter table tb_husband add constraint uk_husband_wifeid 
unique (wifeid);
```

##### 1-N

- 外键约束   --  用父表的列来 约束在子表上

```mysql
--            子表            
alter table tb_order add constraint fk_order_userid 
foreign key (userid) references tb_user (username)
on delete restrict; --            父表    父表的列
```

##### M-N

- 外键约束作用在中间表上

- 中间表 + 外键约束 + 外键约束 

```mysql
-- 中间表
create table tb_record
(
recid int not null auto_increment,
rid int not null,
bid int not null,
borrowdate datetime not null,
returndate datetime,
primary key (recid)
);

alter table tb_record add constraint fk_record_rid 
foreign key (rid) references tb_reader (readerid);

-- 外键约束:  保证参照完整性
alter table tb_record add constraint fk_record_bid 
foreign key (bid) references tb_book (bookid);
```





