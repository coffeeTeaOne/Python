

#### DQL - 数据查询语言

关键字 `SELECT / as / from /( =  <>  <  > )/ is null / is not null/ distinct (去重)/ order by...[asc  desc] /group by ... having / where (条件) /inner join... on / left outer join ... on / right outer join ... on/ full outer join ... on /  limit (偏移量),(查询条数) / limit offset (查询条数), (偏移量)` 

`low_priority`  低优先级  (比如在建表后插入数  (insert into)时 , 用降低优先级的形式最后插入数据)

`concat(a, ":" b)`  将属性 a , b两个字符串属性拼接输出

查多张表时,不写连接条件会查到笛卡尔积



#### 筛选

- = / <> / > / < / is null / is not null   
- 范围 `between ..  and..  `
- 模糊查询`  like `   通配符(`wildcard`)`%  _ `   % 0或任意个字符   下划线表示一个字符
- 去重  ` distinct`
- 排序 ` order by .. `  [asc / desc      升序- 小-大/ 降序 大-小]     第一排序关键字第二 ,第三
- 占位/模糊查询 /正则查询
  * `select * from tb_student where sname like '杨_';`  下划线表示一个占位  
    * `杨%` 以杨开头任意多字符
    * `%杨%` 中间有杨的字符串
    * `regexp '^[杨张]'  `正则筛选
    * `select * from tb_student where sname regexp '^[杨张]'`
  * 判断是否为空要用 `is null /  is not null`
  * 规则  一般是先筛选再排序



#### 聚合函数

- `max() / min () / avg () / sum() / count ()`
- 聚合函数一般会自动排除空值
  - `select min(sbirth) from tb_student where saddr is not null and ssex=1;`
  - `select count(stuid) from tb_student where ssex=1;`



#### 分组查询/筛选

* ` group by... `   一般和聚合函数一起使用

* 在使用 group by 时如果不希望执行默认的排序行为, 使用 order by null 取消排序行为,提升程序的执行性能

- `group by ` 分组筛选用 `having `  加筛选条件

```mysql

select cid avg(score) from tb_sc group by cid having avg(score)>60;

select cid if(ssex, '男', '女') as 性别, count(*) as 人数 from tb_student
where saddr is not null group by ssex order by ssex desc
order by sbirth desc;  -- 排序  desc 降序排序 asc是默认排序 
```



#### 子查询

- 另外一个查询的结果作为其他查询的一个筛选条, 通常可看成是筛选查询的一种

- `select sname, sbirth from tb_student where sbirth=(select min(sbirth) from tb_student);`  单个子查询可以用 =,  多个要用 in 集合运算

```mysql

select sname from tb_student
where stuid in
(
select sid from tb_sc
group by sid having count(sid) > 2;
) and sex=1;  -- 写在右边(后面)的条件先执行
```



#### 连接查询

1. 自然连接查询
2. 内/ 左外/ 右外/ 全 连接查询

**自然连接查询**

- 多个表同时查询  where条件  and 并列条件
- `select sid, cid, score from tb_sc, tb_student, tb_course where sid=stuid and cid=courseid;`
- `select sname, cname, score from tb_sc ,tb_student, tb_course where sid=stuid and cid=courseid;`

**内连接**

* 使用语句`inner join ... on`
* `表1 inner join 表2  on (两个表的连接条件)`相当于将表2加入到表1里面, 用`on`的条件将两个表对应的行一一对应起来

```sql lite

-- 自然连接写法
select sname, avgScore from tb_student t1,
(select sid, avg(score) as avgScore from tb_sc 
 group by sid) t2 where stuid=sid;
 
 -- 内连接写法
 select sname, avgScore from tb_student t1
 inner join (select sid, avg(score) as avgScore 
             group by sid) t2
on stuid=sid;
             
```



#### 左外连接查询

- 查询语句`left outer join ... on `
- 自然连接条件变为  `stuid=sid(+)  ` 也表示左外连接  ` MySQL`是不支持这种语法的
- 左表不满足筛选条件的的记录也会查询出来 ,会将**`null `**也会查找出来
- `left outer join ... on`  

```mysql

select sname, sCount as sCount 
from tb_student t1
left outer join
(select sid, count(sid) as sCount from tb_sc
group by sid) t2 on t1.stuid=t2.sid;
王大锤	4
骆昊	3
杨飘飘	null
张三丰	2
```



#### 右外连接

- 将右表的null也会查询上来 ,实际用法和 左外连接相同
- `right outer join ...  on`



#### 全外连接 

- 左右表不满足连表条件的都会查询上来 (补null) 
- `MySQL`不支持 全外连接
- `full join ... on`



#### 分页查询

- 每种数据库的语法都是不同的
- `limit / limit offset`
- 排序在分页前

```mysql

select sname, cname, score from tb_sc
inner join tb_student on sid=stuid
inner join tb_course on cid=courseid
order by score desc  -- 排序在分页前
limit 5;   -- --  默认偏移量是0  只5条数据
limit 0, 5;  -- 前面是查询开始的偏移量,后面是查询的条数
limit 5 offset 0; -- 前面是查询的条数, 后面是查询的开始偏移量
```



代码下载--[coding代码仓库](https://coding.net/u/zhangminglu/p/exampleCode/git/blob/master/db_demo/select.sql?public=true)

