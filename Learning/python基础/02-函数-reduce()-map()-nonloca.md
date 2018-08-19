

- ### 介绍

主要讲述了一些函数的用法

- reduce()
- map()
- nonlocal



### 1. reduce()

```python
reduce   把一个函数作用在一个序列上, 这个函数必须接受两个参数, reduce 把结果和序列的写一个元素做累积计算
# 运用的是递归的思想  不同之处在于 它是将第一次调用函数的结果作为了第二次调用函数的第一个参数, 
reduce(f, [x1, x2, x3, x4, x5])  = f(f(f( f(x1, x2), x3),x4), x5)
```





### 2. map()

```python
map() 得到的是一个 object 需要进行其他的实体化操作才能得需要的值
>>>def square(x) :            # 计算平方数
...     return x ** 2
... 
#  传入的是函数名 不带()
>>> map(square, [1,2,3,4,5])   # 计算列表各个元素的平方
[1, 4, 9, 16, 25]

# 传入的时候一个匿名函数  
>>> map(lambda x: x ** 2, [1, 2, 3, 4, 5])  # 使用 lambda 匿名函数
[1, 4, 9, 16, 25]
 
# 提供了两个列表，对相同位置的列表数据进行相加, map 会自动查找需要是参数
>>> map(lambda x, y: x + y, [1, 3, 5, 7, 9], [2, 4, 6, 8, 10])
[3, 7, 11, 15, 19]
```



### 3. nonlocal

```python
nonlocal:
    用来在函数或其他作用域中使用外层(非全局)变量
    def scope_test():
    def do_local():
        spam = "local spam" #此函数定义了另外的一个spam字符串变量，并且生命周期只在此函数内。此处的spam和外层的spam是两个变量，如果写出spam = spam + “local spam” 会报错
    def do_nonlocal():
        nonlocal  spam        #使用外层的spam变量
        spam = "nonlocal spam"
    def do_global():
        global spam
        spam = "global spam"     # 输出为nonlocal中的spam???
    spam = "test spam"
    do_local()
    print("After local assignmane:", spam)   # test spam
    do_nonlocal()
    print("After nonlocal assignment:",spam)   #nonlocal spam
    do_global()
    print("After global assignment:",spam)   # nonlocal spam

scope_test()
print("In global scope:",spam)

########################################2222
def make_counter(): 
    count = 0 
    def counter(): 
        nonlocal count 
        count += 1 
        return count 
    return counter 

def make_counter_test(): 
  mc = make_counter() 
  print(mc())
  print(mc())
  print(mc())

make_counter_test()

output:
1
2
3
    
```

##  

```python
集合:	
    
nolocal:
    上一层函数中的变量引用申明
闭包:   
    延长了参数的生命周期  时期参数值变化	
    	def make_counter():
            count = 0
            def counter():
                nonlocal count
                count += 1
                return count
            return counter

        mc = make_counter()
        print(mc())   #1
        print(mc())   #2
        print(mc())   #3
        

config:   (中文释义:配置,布局,显示配置信息)

assert:
    只有满足其后面的条件程序才能向下执行
   		应用:
            通常情况传递参数不会有误，但编写大量的参数检查影响编程效率，而且不需要检查参数的合法性。
            排除非预期的结果。

_ str _

randrange(num):
    在0 - num 范围内随机去值   相当于 randint(range(num))

     
enumerate():   
            将一个可以遍历的数据对象(列表,元组 , 字符串 ) 组合成一个索引序列 , 同时给出数据和下标  默认下标为0 开始
            可设置start=num  规定其开始的下标
            在写 for 循环是增加一个参数 i 
    
            >>>seq = ['one', 'two', 'three']
            >>>for i, element in enumerate(seq):
            ...    print(i, seq[i])
            ... 
            0 one
            1 two
            2 three
            >>>
            >>>seasons = ['Spring', 'Summer', 'Fall', 'Winter']
            >>>list(enumerate(seasons))
            [(0, 'Spring'), (1, 'Summer'), (2, 'Fall'), (3, 'Winter')]
            >>>list(enumerate(seasons, start=1))       # 小标从 1 开始
            [(1, 'Spring'), (2, 'Summer'), (3, 'Fall'), (4, 'Winter')]

```

##