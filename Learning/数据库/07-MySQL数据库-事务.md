

**事务**  -  (transaction / tx)

- 原子性操作性(不可以分割的操作) - 要么全做, 要么全不做

**事务的特点**  -  ACID 特性

- A - atomicity  原子性 : 不可分割, 要么成功要么全失败
- C - Consistency  一致性:  事务前后数据状态要保持一致, 总数一致
- I - Isolation -  隔离性 : 多个事务不能看到对方的中间状态(提交或者回滚之前的状态)
- D - Duration 持久性: 事务完成后数据要持久化(事务的影响要反映在物理存储上)



**操作方法**

begin; -开始事物

commit; - 提交事物   Python 默认是取消自动提交的

rollback; - 回撤操作, 只要操作没有执行 commit 就可以进行回滚操作, 撤回

```mysql
create table tb_account
(
accid char(4) not null,
uname varchar(20) not null,
balance float default 0
)
insert into tb_account values
('1111', '张明禄', 1200.99),
('2222', '王大锤', 500);
-- 开启一个事物   start transaction
begin;
update tb_account set balance=balance-1000 
where accid='1111';
update tb_account set balance=balance+1000 
where accid='2222';
commit;   -- 提交 才能改变
rollback;  -- 撤销

begin;
delete from  tb_account;  -- 没有commmit 不会删除表
rollback;
```



**SQL 注射攻击**

```python
def main():
    config = {
        'host': 'localhost',
        'user': 'root',
        'passwd': '123456',
        'db': 'hrs',
        'charset': 'utf8',
        'cursorclass': pymysql.cursors.DictCursor
    }
    conn = pymysql.connect(**config)
    try:
        uid = input('用户名: ')
        pwd = input('密码: ')
        with conn.cursor() as cursor:
            # 注射攻击的万能密码: a' or '1'='1
            """
            sql = "select 'x' from tb_user where username='%s' \
                     and userpass='%s'" % (uid, pwd)
            if cursor.execute(sql) > 0:
            """
            # cursor.callproc('sp_dept_avg_sal', ())
            # 定义存储过程 / PyMySQL调用存储过程
            if cursor.execute(
                    'select 1 from tb_user where username=%s and userpass=%s',
                    (uid, pwd)):
                print('登录成功, 开始使用系统')
            else:
                print('用户名或密码错误')
    finally:
        conn.close()
```

- 守护进程

  - 守护进程（Daemon Process），也就是通常说的 Daemon 进程（精灵进程），是 Linux 中的后台服务进程。它是一个生存期较长的进程，通常独立于控制终端并且周期性地执行某种任务或等待处理某些发生的事件。

    守护进程是个特殊的孤儿进程，这种进程脱离终端，为什么要脱离终端呢？之所以脱离于终端是为了避免进程被任何终端所产生的信息所打断，其在执行过程中的信息也不在任何终端上显示。由于在 linux 中，每一个系统与用户进行交流的界面称为终端，每一个从此终端开始运行的进程都会依附于这个终端，这个终端就称为这些进程的控制终端，当控制终端被关闭时，相应的进程都会自动关闭。

- 哈希

  - Hash，一般翻译做“散列”，也有直接音译为“哈希”的，就是把任意长度的[输入](https://baike.baidu.com/item/%E8%BE%93%E5%85%A5/5481954)（又叫做预映射， pre-image），通过散列算法，变换成固定长度的[输出](https://baike.baidu.com/item/%E8%BE%93%E5%87%BA)，该输出就是散列值。这种转换是一种压缩映射，也就是，散列值的空间通常远小于输入的空间，不同的输入可能会散列成相同的输出，所以不可能从散列值来确定唯一的输入值。简单的说就是一种将任意长度的消息压缩到某一固定长度的[消息摘要](https://baike.baidu.com/item/%E6%B6%88%E6%81%AF%E6%91%98%E8%A6%81)的函数。