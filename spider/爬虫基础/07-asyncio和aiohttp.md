



#### [官方文档](https://hubertroy.gitbooks.io/aiohttp-chinese-documentation/content/aiohttp%E6%96%87%E6%A1%A3/ClientUsage.html#%E4%BD%BF%E7%94%A8WebSockets)

<http://aiohttp.readthedocs.io/en/stable/>

### asyncio

asyncio 是 Python 3.4 版本引用的标准库， 直接内置了对异步IO 的支持



asyncio的编程模式就是一个消息循环，我们从asyncio模块中直接获取一个 EventLoopd的引用， 然后把需要执行的协程扔到 EventLoop 中执行， 就实现了异步 IO，异步 IO 不会中断 CPU ，CPU 可以 继续其他的请求



#### `@asyncio.coroutine`和`anync + await`效果相同， 只是写法不同

`async`和`await`是针对coroutine(/,kəuru:'ti:n/)的新语法（最新添加的保留关键字），要使用新的语法，只需要做两步简单的替换：

1.  把`@asyncio.coroutine`替换为`async`
2.  把`yield from`替换为`await`。

下面的函数得到相同的结果

```python

import asyncio


@asyncio.coroutine
def hello(n):
    print('hello, world! ' + n)
    r = yield from asyncio.sleep(3)  # 等待 3s 但是程序马上启动了第二个任务
    print('hello complete! ' + n)

async def hello(n):
    print('hello, world! ' + n)
    r = await asyncio.sleep(3)
    print('hello complete! ' + n)

loop = asyncio.get_event_loop()
task = asyncio.wait([hello('AAAAAA'), hello('BBBBBB')])
loop.run_until_complete(task)
loop.close()

```



我们可以在耗时较长的任务前中添加 `await`来让其实现多线程的并发

实际就是在 判断为耗时较长的 程序代码前 添加 `await`



##### 使用自定义域名服务器

### aiohttp

异步 http 请求

底层需要[aiodns](https://aiohttp.readthedocs.io/en/stable/glossary.html#term-aiodns)支持:

```python
from aiohttp.resolver import AsyncResolver

resolver = AsyncResolver(nameservers=["8.8.8.8", "8.8.4.4"])
conn = aiohttp.TCPConnector(resolver=resolver)
```





## 为TCP sockets添加SSL控制:

默认情况下aiohttp总会对使用了HTTPS协议(的URL请求)查验其身份。但也可将*verify_ssl*设置为`False`让其不检查:

```python
r = await session.get('https://example.com', verify_ssl=False)
```

如果你需要设置自定义SSL信息(比如使用自己的证书文件)你可以创建一个**ssl.SSLContext**实例并传递到**ClientSession**中:

```python
sslcontext = ssl.create_default_context(
   cafile='/path/to/ca-bundle.crt')
r = await session.get('https://example.com', ssl_context=sslcontext)
```



## 代理支持

aiohttp 支持 HTTP/HTTPS形式的代理。你需要使用*proxy*参数:

```python
async with aiohttp.ClientSession() as session:
    async with session.get("http://python.org",
                           proxy="http://some.proxy.com") as resp:
        print(resp.status)
```

同时支持认证代理:

```python
async with aiohttp.ClientSession() as session:
    proxy_auth = aiohttp.BasicAuth('user', 'pass')
    async with session.get("http://python.org",
                           proxy="http://some.proxy.com",
                           proxy_auth=proxy_auth) as resp:
        print(resp.status)
```

也可将代理的验证信息放在url中:

```python
session.get("http://python.org",
            proxy="http://user:pass@some.proxy.com")
```

与`requests(另一个广受欢迎的http包)`不同，aiohttp默认不会读取环境变量中的代理值。但你可以通过传递`trust_env=True`来让**aiohttp.ClientSession**读取*HTTP_PROXY*或*HTTPS_PROXY*环境变量中的代理信息(不区分大小写)。

```python
async with aiohttp.ClientSession() as session:
    async with session.get("http://python.org", trust_env=True) as resp:
        print(resp.status)
```





## 设置超时

默认情况下每个IO操作有5分钟超时时间。可以通过给**ClientSession.get**()及其同类组件传递`timeout`来覆盖原超时时间:

```python
async with session.get('https://github.com', timeout=60) as r:
    ...
```

`None` 或者`0`则表示不检测超时。 还可通过调用**async_timeout.timeout**上下文管理器来为连接和解析响应内容添加一个总超时时间:

```python
import async_timeout

with async_timeout.timeout(0.001):
    async with session.get('https://github.com') as r:
        await r.text()
```



## 愉快地结束:

当一个包含`ClientSession`的`async with`代码块的末尾行结束时(或直接调用了`.close()`)，因为asyncio内部的一些原因底层的连接其实没有关闭。在实际使用中，底层连接需要有一个缓冲时间来关闭。然而，如果事件循环在底层连接关闭之前就结束了，那么会抛出一个 资源警告: 存在未关闭的传输(通道)(`ResourceWarning: unclosed transport`),如果警告可用的话。 为了避免这种情况，在关闭事件循环前加入一小段延迟让底层连接得到关闭的缓冲时间。 对于非SSL的`ClientSession`, 使用0即可(`await asyncio.sleep(0)`):