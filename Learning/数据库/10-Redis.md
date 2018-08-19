

##### 应用对比,MySQL与Redis

- mysql
  - 适用于高度事务性的系统。例如银行或者会计系统，传统的关系型数据库目前还是更实用于需要大量原子性复杂事务的应用程序
  - 传统的商业智能应用，针对特定问题的BI数据库会对产生高度优化的查询方式，对于此类应用，数据仓库可能是更合适的选择
- Redis
  - 用来做缓存-redis的所有数据时放在内存中的
  - 可以在某些特定应用场景下替代传统数据库--比如社交类的应用
  - 在一些大型系统中，巧妙的实现一些特定的功能：session共享、购物车
  - MongoDB不支持SQL语句



##### 非关系数据库概念的提出

NoSQL - 1998 提出这个概念   不使用关系型数据库

- - 2008/2010 年后进入大数据时代,  Big Data, 
  - No SQL     - - > Not Only SQL   不仅仅适用关系型数据库
- 非关系型数据库的产品
  - Redis   - 高速缓存 (高速将数据从硬盘转移到内存)
    - 键值对
  - MongoDB  -  存放低价值,结构不严谨, 灵活的数据(可以随意添加列)
    - 文档 数据库
    - 重要的数据要保持强一致性(关系型数据库,底层锁结构, 保护数据)
  - Neo4j  -  社交网站, 网状结构数据库, 关联性挖掘 ,人脉关系
    - 图 数据库
  - HBase  - 列族数据库, 将数据放在列里面, 每组数据都在一个列里面



### Redis

经典应用场景: 做高速缓存

没有Redis之前使用的是 memcached ,后来被淘汰, redis和它原理差不多

- 部署在Linux系统中
- `usr/local/bin`

#### 配置Redis

1. 将原先下载的文件目录下的Redis.config,复制一份
2. 进入复制的配置文件
   1. 61 - 绑定真实IP          (ifconfig)
   2. 84 - 更改端口 - port -      防止别人攻击,自定义端口
   3. 93 -队列大小, 可以不改
   4. 480 - 设置口令密码password
   5. 178  databases  16  默认开启16个数据库, 可以自行调整
      1. 隔离数据, 也可以值开1库
3. 启动 redis-server  myredis.config(配置文件重新定向)
4. 关闭进程  -  kill 进程号  加 -9可能会丢失数据

测试`redis-benchmark -h 172.24.31.255 -p 9439 -a qwertyuiop8`

1. 走公网要开端口 - 在阿里云开相应的端口
2. 连接 - `redis-cli -h 39.104.171.126 -p 6379 -a qwertyuiop8`公网IP



##### Redis命令

- set foo hello ex 300  设置生存时间
- ttl foo 查看生存时间
- -1 没设置生存时间, -2 消失
- expire username 15  -  加设生存时间15秒
- shutdown 关机
- save - 保存   bg-save 后台保存
- 切换数据库   select 2切换到2号库
- flushdb 清空数库  清空一个
- flushall  清空所有数据库
- info  查看配置信息
- exists key 检查key是否存在
- keys pattern 查找符合所有给定模式的key
- dbsize    



#### 数据类型

指键值对中值的类型

- 字符串

  - incr   自增

  - decr  减少

  - incrby key  num 增加数字

  - increfloat  浮点增加

  - 投票  - 关系型数据库每次都需要更新, 解决并发访问, 设置了底层锁 ,更新需要排队,使得性能很差,   使用`incr`很好的解决这个问题

  - strlen  查看字符创长度 

  - append username kitty  在见后面追加内容

  - 也可以存`JSON`格式的

  - 将网站首页全部的代码当成字符串存储在对应一个键中, 直接在需要的时候通过查找 key的方式来发送数据, 内存读取, 是的网页快速加载

  - 操作关系型数据库的, SQL 语句缓存起来, 将查询结果缓存, 之前用户的查询结果可以给以后用户的查询时, 直接给给与用户, 二级缓存


- 哈希表

  - 在字典中嵌套字典
  - hset key name 'zhang'
  - hget / hgetall
  - 写入需要写键和值,
  - hdel 删除哈希表中的字段
  - del key 直接将哈希表删除
  - hexists  key判断哈希表中的字段是否存在
  - hincrby 给某个字段增加值



- 列表
  - 元素一个挨着一个, 有索引和下标
  - ``rpush / lpush`` 给列表添加内容
  - `lrange foo start stop(-1到最后一个)` 查看元素
  - `rpop foo / lpop foo `取元素 
  - `lindex` 根据下标查看元素
  - ``linsert foo before 400 999`` 直接写元素, 在哪个元素的前面添加元素
  - `lrem foo 2 999` 指定 2 次删除 999
  - 应用场景: 微博发表文章, 使关注的 人都能看到(使用列表做一个消息队列)
  - 生产者消费者模型(多线程)



- 集合
  - 不能放重复的元素
  - sadd foo 10 20 30 40 50 60 70 30 40 20  给集合放内容
  - smembers foo  查看元素
  - sinter a b  交集
  - sunion a b 并集
  - sdiff a b 差集
  - 删除元素  srem 
  - scard  foo 返回集合的元素的个数



- 有序自合  zset 
  - zadd 加元素   zadd foo (排名) 值  (排名) 值   100 hello  200 world
  - zrange foo 0 -1  正序
  - zreverange foo 0 -1 反序
  - 更新排名 zincrby foo 400 hello



- Python 操作Redis

```python
import redis


def main():
    client = redis.Redis(host='39.104.171.126', port='6379', password='psw')
    if client.ping():
        print(client.keys("*").decode('utf-8'))   #得到的是二进制数据, 需要解码
        # print(client.get('username').decode('utf-8'))
        # print(client.get('name').decode('utf-8'))
        print(list(map(lambda x: x.decode('utf-8'),
                       client.zrange('foo', start=0, end=-1))))
        print([x.decode('utf-8')
               for x in client.zrange('foo', start=0, end=-1)])


if __name__ == '__main__':
    main()
```



- 读写分离
  - 一台 master 写操作   三台  slave(奴隶)读取  实验
  - slaveof [IP]    120.77.222.217 11223  给该IP 做slave
  - info replication  查看连接情况查看
  - 更改 config 文件   265 写入`` slaveof IP port ``  272   写入 master密码
  - cp myredis.config  更改端口可以在一个电脑上开多个redis服务器



- master 停止服务时 , 让其他的slave 扮演 master 
  - 让自动完成  哨兵   `sentinel.config	`
    - 更改 `sentinel.config`     复制一份更改配置
      - `15 - bind 内网地址`       绑定内网地址 ,访问用公网地址 
      - `17 -  protect-mode yes`
      - 70 行`sentinel monitor master + IP + port 2(票数)` 开端口
      - 73 - 改名字和密码  master
    - 启动哨兵
      - `redid-server sentinel.conf  --sentinel `
      - 更改哨兵配置
        - `bind 内网IP`
        - `port `
        - `sentinel monitor mymaster  master公网IP port 哨兵数`
        - `sentinel pass mymaster <password>`
        - `sentinel down-after-milliseconds mymaster 3000`   开始投票反应时间
        - `sentinel failover-timeout myster 180000`   设置返回时间, 规定时返回还是master
    - 脚本  - -LUA    定自定义新的命令    
    - 集群   --Cluster   分区操作, 将多个节点变为一个节点 , 轮转
      - `yum install ruby`



