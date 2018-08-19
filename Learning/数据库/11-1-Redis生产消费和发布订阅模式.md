Redis消息队列

http://python.jobbole.com/84039/

生产消费模式和发布订阅模式



**生产消费模式**

- 写入：rpush
- 读取：blpop   阻塞读取，如果队列没有数据就阻塞等待，监听队列

**发布订阅模式**

```python

# 创建 频道 channel 进行发布消息
redis > publish channel_name 'message_body'

# 订阅频道，接收消息，会一直等待
redis > subscribe channel_name


```



