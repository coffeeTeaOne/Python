

### csv文件



```python

with open('./file/MontyPythonAlbums.csv', 'r') as f:
    with open('test.csv', 'w') as t:

        csvReader = csv.reader(f) # 读取文件
        for row in csvReader:
            print(row)
            writer = csv.writer(t)
            # 写入文件  注意的是这里的 row 是一个列表， 里面是一行所有字段的信息
            writer.writerow(row)
            # 写入多行
            # writer.writerows(row1, row2, row3)
```





### PDF文件

-   `PDFMiner3K` 库可以操作 PDF 文件, 适用于 python3.x版本
-   `pip install PDFMiner3K`



#### 读取PDF文件

```python

from pdfminer.pdfinterp import PDFResourceManager, process_pdf
from pdfminer.converter import TextConverter
from pdfminer.layout import LAParams
from io import StringIO


def readPDF(pdfFile):
    rsrcmgr = PDFResourceManager()
    retstr = StringIO()
    laparams = LAParams()
    # 默认 pageno=1
    device = TextConverter(rsrcmgr=rsrcmgr, outfp=retstr, laparams=laparams)
    
    process_pdf(rsrcmgr=rsrcmgr, device=device, fp=pdfFile)

    device.close()
    content = retstr.getvalue()
    retstr.close()
    return content

```





### word 文档

.doc  .docx 文档

python-docx库， 只是支持创建文档和读取一些文档的基本数据， 入文件的大小和标题， 不支持正文的读取



##### 代码- 正则匹配读取， 还可以使用 bs4 和 lxml 读取

```python

import re
from zipfile import ZipFile
from io import BytesIO, StringIO


def readDocx(filePath):
    """
    读取 .docx 文件
    
    :param filePath: 文件路径 
    :return:  文件内所有内容的列表
    """
    with open(filePath, 'rb',) as f:
        wordFile = f.read()
        # 二进制文件
        wordFile = BytesIO(wordFile)
        # 因为所有的 .docx 文件都是经过压缩的， 所以需要进行解压
        document = ZipFile(wordFile)
        # 读取文件， 指定文件的格式为 xml
        xml_content = document.read('word/document.xml')
        # 正则匹配 需要的内容
        content_str = xml_content.decode('utf-8')
        pattern = re.compile(r'<w:t[>](.*?)</w:t>', re.S)
        content_list = re.findall(pattern, content_str)
        return content_list
```



bs4 和 xpath 读取 - 未能抓取到需要的 `<w:t></w:t>` 元素

失败代码

```python
def readDocx(wordFile):
    """
    读取 .docx 文件的内容
    :param wordFile:  目标文件
    :return: 文档的文字内容
    """
    content = StringIO()
    # 获取网络文档
    wordFile = BytesIO(wordFile)
    # 因为所有的 .docx 文件都是经过压缩的， 所以需要进行解压
    document = ZipFile(wordFile)
    # 读取文件， 指定文件的格式为 xml
    xml_content = document.read('word/document.xml')

    # wordObj = BeautifulSoup(xml_content.decode('utf-8'), 'lxml')
    # 所有的正文内容都在 <w:t> 标签中， 标题也是一样， 标题只是在外面套了一层标签
    # textStrings = wordObj.findAll("w:t")

    # # 通过 etree 获取  传入 bytes 对象即可
    # wordObj = etree.HTML(xml_content)
    # textStrings = wordObj.xpath('//*w:t')

    # 正则匹配
    content = xml_content.decode('utf-8')
    pattern = re.compile(r'<w:t[>](.*?)</w:t>', re.S)
    textStrings = re.findall(pattern, content)
    print(textStrings)
    for textElem in textStrings:
        c = textElem
        print(c)

    # return content
```

