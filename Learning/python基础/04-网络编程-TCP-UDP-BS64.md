 



## 1. 网络介绍

- 多台独立自主的计算机互联的总称叫计算机网络
- 实现信息互联与资源共享 
- 网络接口 -->  网络  -->  传输  --> 应用 
- Inernet  因特网   基于TCP/IP Model 的网络 
- ​

### 1. 1 TCP   可靠传输协议    

- 传输控制协议  Transfer Control Protocol
  - 可靠通信(数据不会穿丢也不会传错)
  - 流量控制  自动调节发送数据的速度
  - 拥塞控制  网络拥堵时会降低发送速率
  
- 可靠通讯的实现:
  -  如何保证数据不会传输错误
  -  握手机制 +    在数据中添加冗余校验对发送的数据  进行校验
- 流量控制:
  -  滑动窗口机制   逐步增加发送的数据大小        

- 拥塞控制:
  - 减小滑动窗口  减小发送速率

### 1.2 UDP  

- User Datagram Protocol 用户数据宝协议
  -  数据可能会丢失某部分内容 只要不影响使用  列如视屏  不会影响用户的使用 

## 2.  应用实例  网络扒图片

- 通过天行数据提供的数据接口

```python
import os
import requests, json


def get_file(path):
    """
    在url中查找图片

    :param path:
    :return:   查找结果
    """
    resp = requests.get(path)
    url_dict = json.loads(resp.text, encoding='utf-8')
    i = 0
    for new_dict in url_dict['newslist']:
        url = new_dict['picUrl']
        resp_new = requests.get(url)
        print(resp_new.content)
        i += 1
        with open ('%d.jpg' % i, 'wb') as f:
            f.write(resp_new.content)
            print(i)
    print('下载完成')


def rm_picture(path):
    """
    删除jpg格式文件

    :param path: 删除的文件夹
    """
    list_file = os.listdir(path)
    for file in list_file:
        if file[file.rindex('.') + 1:] == 'jpg':
            os.remove(file)
    print('图片删除完毕')


path_file = '../day18'
path_url = 'http://api.tianapi.com/meinv/?key='
def main():
    # get_file(path_url)
    rm_picture(path_file)


if __name__ == '__main__':
    main()
```



## 3. 网络编程  服务器  客户端

### 3 .1 套接字编程   socket()



- 服务器与客户端  代码实现实例

```python
********     服务器        #### 套接字编程  socket()
from socket import socket


def servey():
    ser_temp = socket()
    ser_temp.bind(('10.7.152.105', 7779))
    ser_temp.listen()
    while True:
        client, addr = ser_temp.accept()
        while True:     #  必须有这个循环才能一直接受一个客户的消息, 没有循环只能接收一次
            get_msg = client.recv(1024).decode('utf-8')
            print(get_msg)
            sed_msg = input('服务器:').encode('utf-8')
            client.send(sed_msg)


 ***********   客户端  ***********************         
from socket import socket


def client():
    client = socket()
    client.connect(('10.7.152.105', 7779))
    while True:
        msg = input('客户端:').encode('utf-8')
        client.send(msg)
        get_msg = client.recv(1024).decode('utf-8')
        print(get_msg)
```

### emample  用服务器与客户端结合实现猜数字游戏

```python
#服务器
from random import randint
from socket import socket
from time import sleep


class Robot(object):
    def __init__(self):
        self.answer = randint(1, 100)
        self._hint = ''
        self.count = 0

    @property
    def hint(self):
        return self._hint

    def judge(self, yours):
        self.count += 1
        if yours > self.answer:
            self._hint = '请输入小一点'
        elif yours < self.answer:
            self._hint = '请输入大一点'
        else:
            self._hint = '猜对了,但没有奖励....'
            if self.count > 7:
                self._hint += '\n智商真捉急!!!'
            return True
        return  False


def main():
    robot = Robot()
    server = socket()
    server.bind(('10.7.152.105', 9669))
    server.listen(512)
    print('服务器开启...')
    while True:
        client, addr = server.accept()
        print(addr)
        while True:
            get_msg = int(client.recv(1024).decode('utf-8'))
            if 0 < get_msg < 100:
                robot.judge(get_msg)
                out_msg = robot.hint
                client.send(out_msg.encode('utf-8'))
            else:
                out_msg = '输入有误,重新输入'
                client.send(out_msg.encode('utf-8'))

    print('服务器关闭.....')
    server.close()


if __name__ == '__main__':
    main()
    
    
    \****************************************\
    # 客户端

from socket import socket


def main():
    client = socket()
    client.connect(('10.7.152.105', 9669))
    while True:
        numstr = input('输入数字:')
        client.send(numstr.encode('utf-8'))
        in_msg = client.recv(1024).decode('utf-8')
        print(in_msg)

    print('客户端断开连接')
    client.close()


if __name__ == '__main__':
    main()
```

## 4. BASE 64 编码

```python
将二进制数据变为有64种符号表示的文本
011011001100001111010011
将二进制编码每6位分开
```

### 4.1 处理方法  示例

```python
用一个字典的 键值对 形式保存要发送的数据
将字典处理成 JSON 格式进行传输

JSON 只能是纯文本   用 BSAE64 处理二进制数据 
    dumps()处理二进制数据
  
\***********************       服务器
from socket import socket, SOCK_STREAM, AF_INET
from base64 import b64encode
from json import dumps


def main():
    # 1.创建套接字对象并指定使用哪种传输服务
    server = socket()
    # 2.绑定IP地址和端口(区分不同的服务)
    server.bind(('10.7.152.69', 5566))
    # 3.开启监听 - 监听客户端连接到服务器
    server.listen(512)
    print('服务器启动开始监听...')
    with open('memory.png', 'rb') as f:
        # 将二进制数据处理成base64再解码成字符串
        data = b64encode(f.read()).decode('utf-8')
    while True:
        client, addr = server.accept()
        # 用一个字典(键值对)来保存要发送的各种数据
        # 待会可以将字典处理成JSON格式在网络上传递
        my_dict = dict({})
        my_dict['filename'] = 'memory.png'
        # JSON是纯文本不能携带二进制数据
        # 所以图片的二进制数据要处理成base64编码
        my_dict['filedata'] = data
        # 通过dumps函数将字典处理成JSON字符串
        json_str = dumps(my_dict)
        # 发送JSON字符串
        client.send(json_str.encode('utf-8'))
        client.close()


if __name__ == '__main__':
    main()
**********************************************
##   客户端  #############
from socket import socket
from json import loads
from base64 import b64decode


def main():
    client = socket()
    client.connect(('10.7.152.69', 5566))
    # 定义一个保存二进制数据的对象
    in_data = bytes()
    # 由于不知道服务器发送的数据有多大每次接收1024字节
    data = client.recv(1024)
    while data:
        # 将收到的数据拼接起来
        in_data += data
        data = client.recv(1024)
    # 将收到的二进制数据解码成JSON字符串并转换成字典
    # loads函数的作用就是将JSON字符串转成字典对象
    my_dict = loads(in_data.decode('utf-8'))
    filename = my_dict['filename']
    filedata = my_dict['filedata'].encode('utf-8')
    with open('c:/images/' + filename, 'wb') as f:
        # 将base64格式的数据解码成二进制数据并写入文件
        f.write(b64decode(filedata))
    print('图片已保存.')


if __name__ == '__main__':
    main()


```



