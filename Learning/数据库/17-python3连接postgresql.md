python3连接postgresql

```python
import psycopg2


host = '127.0.0.1'
port = 5432
user = 'postgres'
password = '123456'
dbname = 'test'


conn = psycopg2.connect(dbname=dbname, user=user,
                        password=password, host=host, port=port)

cur = conn.cursor()

cur.execute('select * from testcase;')

result = cur.fetchall()
print(result)
```

