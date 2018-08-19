

```python

from io import StringIO


s = StringIO()
s.write('www.baidu.com\n')
s.write('abc\n')
s.write('zhang')


o = s.getvalue()
print(o)


s.seek(0)  # 指定开始读取的位置
while True:
    strBuf = s.readline()
    if not strBuf:
        break
    print(strBuf, end='')

s.close()


# 另外还有  readline()  readlines()  等方法可以读取
```

