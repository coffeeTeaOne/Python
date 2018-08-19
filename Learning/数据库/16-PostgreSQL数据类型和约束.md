16-PostgreSQL数据类型和约束



#### **数据类型**

```python

Boolean : true 1 / false 0 / null
Character : char / varcahr / text
Numeric : integer / floating-point
Timeporal : date / time / timestamp（时间戳） / interval(间隔)
UUID : 唯一标识符 UUID
Array : 存储字符串，数字等
JSON : 
hstory : 存储键值对
特殊类型 ： 如网络地址和几何数据

```

#### **Character**

- `char(n)`  定常字符
- `varchar(n)`  不定长字符
- `text`  可变长字符，理论上不限制长度，但是默认设置的是每行 不超过 8K

#### **Numeric**

- `SMALLINT` 2字节有符号整数
- `int` 4 字节   还有 int8 
- `float(n)`   
- `real`或`float8` 双精度浮点数
- `numeric(p,s)`p 位实数，小数点后带s位小数

#### **特殊类型的数字**

- box    矩形框
- line   一组积分
- point    一对几何数字，点坐标
- lseg   线段
- polygon  封闭几何形状
- inet  IP4地址
- macaddr   一个 MAC 地址

#### 列约束

- NOT NULL
- UNIQUE
- PRIMARY KEY
- REFERENCES 定义外键约束
- CHECK  更新数据是的检查条件，例如，插入数据是必须为正
  - `CHECK(condition)`

**SELECT INTO**

- 将查找出的内容创建一个新表

```sql
-- 将film表中查询中的内容创建一个信标film_r
select film_id,title, rental_tate into table film_r
from film where rating="R" and rental_duration=5 order by title;
```

#### **CREATE AS**

```sql
-- 将查询的结果作为一个  temp 临时表，unlogged 一个未记录的新表
CREATE [TEMP UNLOGGED] TABLE new_table_name as query;

CREATE TABLE new_table_name (column_name_list) as query;

CREATE TABLE IF NOT EXISTS new_table_name as query;
```

#### **更改表中的行和列的名称**

- http://www.postgresqltutorial.com/postgresql-alter-table/

检查约束和非空约束

```sql
create table table_name (
    id serial primary key,
    product_id int not null,
    qty numeric not null check(qty > 0),
    net_price numeric check(net_price > 0)
);
```

#### 主键约束

```sql
-- 添加主键
alter table table_name ADD COLUMN ID SERIAL PRIAMRY KEY;
-- 删除主键
alter table table_name DROP CONSTRAINT primary_key_constraint;
```



#### **外键约束**

```sql
create table so_headers(
	id serial primary key,
    customer_id integer,
    ship_to varchar(255)
);

-- 创建外键约束的表
create table so_items (
	item_id integer not null,
    so_id integer references so_headers(id),
    product_id integer,
    qty integer,
    primar key (item_id, so_id)
);

-- 也可以这样
create table child_table (
	c1 integer primary key,
    c2 integer,
    c3 integer,
    FOREIGN KEY(c2, c3) REFERENCES parent_table (p1, p2)
);

-- alter 添加
 alter table child_table ADD CONSTRAINT constraint_name 
 FOREIGN KEY (c1) REFERENCES parent_table(p1) ON DELETE CASCADE;
 
 -- 删除外键约束
 ALTER TABLE child_table DROP CONSTRAINT constraint_fkey;
 
```

