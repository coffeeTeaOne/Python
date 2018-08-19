### beautifulSoup



#### 安装包

-   `pip install beautifulsoup4`


#### 导入包

-   `form bs4 import BeautifulSoup`




### 1. 基本用法

beautifulSoup得到的是一个 bsObj  我们可以在它的基础上进行获取我们需要的信息

```python


from urllib.request import urlopen
from bs4 import BeautifulSoup

html = urlopen('')
bsObj = BeautifulSoup(html.read())
print(bsObj.h1)  # 获取 h1 标签

# 获取 标签中的 class='green'的 span
span = bsObj.findAll('span', {'class': 'green'})

# 获取文本信息
span.get_text()

# 获取属性值
span.attrs.get('attr')
span.get('attr')


```



>   在获取文本信息时, 会将所有的标签信息都清除, 应该尽量避免这样, 尽量保存 HTML 文档的标签结构



#### find() 和 findAll()

-   `findAll(tag, attributes, recursive, text, limit, keywords)`
-   `find(tag, attributes, recursive, text, keywords)`

**参数解释**

-   `tag` - 可以传入一个或者多个标签名称组成的列表来查找, 将返回所有的包含要查找的标签的列表集合
-   `attributes` - 传入一个字典格式的属性和属性对应的值`{'class', 'green'}`
-   `recursive` - 递归参数, 是一个布尔变量,   ==默认==为 `True` , 会查找需要的子标签以及子标签的子标签,  `False` 只会查找文档的一级标签
-   `limit` - 只用于 `findAll` 方法, 获取需要的前几项, 一般是按照页面的顺序来排序的
-   `keyword` - 指定关键字, 可以更加具体的指定属性的标签, 注意使用`class` 是会造成冲突
    -   `bsObj.findAll(id='text')`
    -   `bsObj.findAll('', {'id', 'text'})`



可以直接获取 bsObj 对象的标签

-   `bsObj.div.h1`  获取 h1 标签 - ==只对 bsObj 有效==



`NavigableString`对象用来表示标签里的文字



`Comment` 对象用来表示文档中的注释



### 2. 导航树



`bsObj.div.findAll('img')`  找出文档中的第一个 div 然后获取这个 div 中所有的后代中的 img 标签 列表



#### 获取子标签

```python

# 获得表格中的所有的 子标签 (tr th  td) 的内容
for child in bsObj.find('table', {'id', 'goo'}).children:
    print(child)
```



#### 兄弟标签

`bsObj.next_siblings`  下一个兄弟标签

`bsObj.previous_silblings` 上一个兄弟标签



#### 父标签

`bsObj.parent`  父标签

parents  和  parent 的区别



#### 正则表达式 和 `BeautiflSoup`



>   `[^a-z] `     表示不匹配包含在 `[ ]`中的内容, 例为 不匹配` a-z`
>
>   ` ?! `  **排除**  也表示 不包含,  通常放在 字符或者 正则表达式的前面, 让后面的正则表达式 排除不包含的内容  例  `^(?![A-Z]).)*$` - 匹配`.`但不包含 `A-Z `



```python

import re

from bs4 import BeautifulSoup


...
bsObj = BeautifulSoup(html)
images = bsObj.findAll("img", {"src", re.compile("\.\.\/img\/gifts/img.*\.jpg")})
for image in images:
    print(iamge['src'])
    
```



#### 获取属性

`bsObj.attrs` 获取全部属性, 得到的是一个字典

`bsObj.attrs['src']` 获取  src 属性



#### Lambda 表达式

`soup.findAll(lambda tag: len(tag.attrs) == 2)` 获取满足 属性的个数是 2 的标签





### 3. BeautifulSoup的解析器

##### 3.1 Python标准库

```
使用方法: BeautifulSoup(html_doc,"html.parser")

优势：Python内置，执行速度适中，文档容错能力强

劣势：Python 2.7.3 or 3.2.2)前 的版本中文档容错能力差
123456
```

#### 3.2 lxml解析器(推荐使用)

```
使用方法：BeautifulSoup(html_doc,'lxml')

优势：速度快，文档容错能力强（C编写），推荐使用
1234
```

#### 3.3 html5lib

```
使用方法：BeautifulSoup(html_doc,"html5lib")

优势：最好的容错性，已浏览器的方式解析文档，生成Html5格式的文档

劣势：速度慢，不依赖外部扩展
```























