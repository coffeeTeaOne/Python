

```python
import pymysql


class Foo(object):

    conn = pymysql.connect(host='localhost',
                           port=3306,
                           user='root',
                           passwd='123456',
                           db='msg',
                           charset='utf8',
                           autocommit=False)

    def create_tb(self):
        """
        建表
        """
        with Foo.conn.cursor() as cursor:
            cursor.execute("""
                drop table if exists tb_addrBook;
                create table tb_addrBook(
                pid int auto_increment,
                name varchar(20) not null,
                sex char(5) default '男',
                tel char(11) not null,
                addr varchar(30),
                primary key (pid)
                );""")

    def insert(self):
        """
        插入内容
        :return: 
        """
        temp = True
        while temp:
            name = input('请输入姓名:')
            sex = input('性别(男-1/女-0):')
            if sex == '1':
                sex = '男'
            elif sex == '0':
                sex = '女'
            else:
                temp = False
                print('输入有误, 重新输入!!')
            tel = input('电话号码:')
            addr = input('家庭住址:')
            with Foo.conn.cursor() as cursor:
                cursor.execute("""insert into tb_addrBook (name, sex, tel, addr) 
                                        values(%s, %s, %s, %s)""",(name, sex, tel, addr))
            temp = input('是否还要继续增加? 是(1) 否 (0)' )
            if temp == '0':
                temp = False

    def update(self):
        """
        将电话号码改为 0000000000
        """
        print('输入您要更新的姓名!')
        name = input('姓名:')
        with Foo.conn.cursor() as cursor:
            cursor.execute('update tb_addrBook set tel="00000000000" where name=%s', (name, ))

    def delate(self):
        """
        删除联系人
        """
        print('输入您要删除的姓名!')
        name = input('姓名:')
        with Foo.conn.cursor() as cursor:
            cursor.execute('delete from tb_addrBook where name=%s',(name))

    def select(self):
        """
        查找
        """
        print('输入您要查找的姓名!')
        name = input('姓名:')
        with Foo.conn.cursor() as cursor:
            cursor.execute('select * from tb_addrBook where name=%s', (name))
            b = cursor.fetchone()
            print(b)

    def search(self):
        in_msg = input('输入您要查找的姓名:')
        with Foo.conn.cursor() as cursor:
            msg = '%' + in_msg + '%'
            cursor.execute('select pid, name,sex, tel, addr from tb_addrBook where name like %s', (msg,))
            b = cursor.fetchall()
            print('%10s %10s %15s %8s %10s' % ('pid', 'name', 'sex', 'tel', 'addr'))
            for i in [*b]:
                print('%03s %10s %10s %15s %8s' % (str(i[0]), i[1], str(i[2]), i[3], i[4]))

    def show(self):
        with Foo.conn.cursor() as cursor:
            cursor.execute('select pid, name, sex, tel, addr from tb_addrBook')
            b = cursor.fetchall()
            print('%03s %10s %10s %15s %8s' % ('编号(pid)', 'name', 'sex', 'tel', 'addr'))
            for i in [*b]:
                print('%03s %10s %10s %15s %8s' % (str(i[0]), i[1], str(i[2]), i[3], i[4]))

    def commit(self):
        """提交"""
        Foo.conn.commit()

    def rollback(self):
        """回滚"""
        Foo.conn.rollback()

    def close(self):
        """关闭连接"""
        Foo.conn.close()


def main():
    foo = Foo()
    try:
        foo.create_tb()
        while False:
            foo.show()
            print('增加(1) 修改(2) 查找(3) 删除(4) 退出(5)')
            ope = input('选择:')
            if ope == '1':
                foo.insert()
                foo.commit()
            elif ope == '2':
                foo.update()
                foo.commit()
            elif ope == '3':
                foo.search()
            elif ope == '4':
                foo.delate()
                foo.commit()
            else:
                break
    except:
        foo.rollback()
    finally:
        foo.close()


if __name__ == '__main__':
    main()
```

