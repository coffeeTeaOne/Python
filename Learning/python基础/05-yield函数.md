

### `sys`

- 在Linux系统命令行参数  在输入命令是给的参数 
- `sys.argv`  接受所有的参数  保存在数组中

### `yield`



```python
# 生成式   列表已存在,占用空间大
list1 = [x for x in range(10)]

#生成器    得到的是 generator  对象 引用 
list3 = (x for c in range(10))
for i in list3:   # 在需要用的时候再计算出值
    print(i)
   
# 生成器函数
def fibo(n):  #普通函数 
    a, b = (0, 1)
    for _ in range(n):
        a, b = b, a + b
    return a

def fibo(n):  #生成器函数   保留上次计算的值 不会重复计算 
    a, b = (0, 1)
    for _ in range(n):
        a, b = b, a + b
    	yield a
```

```python
string.center(占据的位置大小, [,空位填补])
string.ljust()
string.rjust()
# 二选一列表
[[0], [1]][True] = [1]
[[0], [1]][False] = [0]

```



#### 进阶

*   生成器函数， 可以理解为暂停，程序会暂停在yield的地方， 等待下一次调用 next() 时， 程序又会执行一次， 然后继续执行

#### next()

可以通过打断点来进行理解， 让程序一步一步执行， 查看程序到底执行到了那里， 暂停到了那里

![53015117120](../01-python%E5%9F%BA%E7%A1%80/assets/1530151171209.png)







#### send()

```python

import time


def constumer():
    r = ''
    while True:
        n = yield r
        if not n:
            return
        print('[CONSUMER] 消费者：%s' % n)
        time.sleep(1)
        r = 'CONSUMER，结束状态！'

def produce():
    c = constumer()
    next(c)

    n = 0
    while n < 3:
        n += 1
        print('n的值：%s...' % n)
        # 将 n 传入到 yield 中， yield r 的值 替换为 n，
        # 同时 将原来的r 的值获取到， 赋值给当前的 r
        r = c.send(n)
        
        print('r的值：%s' % r)
        print('--' * 20)


def main():
    produce()


if __name__ == '__main__':
    main()


```

