scrapy-redis配置布隆过滤器



修改 scrapy-redis -> dupefilter.py 文件

新增部分有注释

```python

class RFPDupeFilter(BaseDupeFilter):

    logger = logger

    def __init__(self, server, key, debug=False):
       
        self.server = server
        self.key = key
        self.debug = debug
        self.logdupes = True
        # 添加
        self.hash_num = hash_num
        self.bf = BloomFilter(server, key, bit, hash_number)

    @classmethod
    def from_settings(cls, settings):
        server = get_redis_from_settings(settings)
        key = defaults.DUPEFILTER_KEY % {'timestamp': int(time.time())}
        debug = settings.getbool('DUPEFILTER_DEBUG')
        
        # 添加
        bit = settings.getint('BLOOMFILTER_BIT', BLOOMFILTER_BIT)
        hash_number = settings.getint('BLOOMFILTER_HASH_NUMBER', BLOOMFILTER_HASH_NUMBER)
        return cls(server, key=key, debug=debug, bit=bit, hash_number=hash_bumber)

    
# 添加内容 ##########################################################

# 测试使用
# BLOOMFILTER_HASH_NUMBER = 6
# BLOOMFILTER_BIT = 30


class HashMap(object):
    """
    遍历 value 的每一位，并利用ord()方法取到每一位的 ASCII 码，然后混淆 seed 进行迭代求和，
    最终得到一个数值。这个数值的结果就由 value 和 seed 唯一确定。
    """
    def __init__(self, m, seed):
        self.m = m
        self.seed = seed

    def hash(self, value):
        ret = 0
        for i in range(len(value)):
            ret += self.seed * ret + ord(value[i])
        return (self.m - 1) & ret


class BloomFilter(object):
	"""布隆过滤器"""
    def __init__(self, server, key, bit=BLOOMFILTER_BIT, hash_number=BLOOMFILTER_HASH_NUMBER):
        self.m = 1 << bit
        self.seeds = range(hash_number)
        self.maps = [HashMap(self.m, seed) for seed in self.seeds]
        self.server = server
        self.key = key

    def exists(self, value):
        if not value:
            return False
        exists = 1
        for map in self.maps:
            offset = map.hash(value)
            exists = exists & self.server.getbit(self.key, offset)
        return exists

    def insert(self, value):
        for f in self.maps:
            offset = f.hash(value)
            self.server.setbit(self.key, offset, 1)

```

defaults.py添加

```python
BLOOMFILTER_HASH_NUMBER = 6
BLOOMFILTER_BIT = 30
```



测试

```python
"""测试 BloomFilter """

import redis


conn = redis.StrictRedis(host='localhost', port=6379, password='')
bf = BloomFilter(conn, 'testbf', 5, 6)
bf.insert('Hello')
bf.insert("world")
result = bf.exists('Hello')
print(bool(result))  # True

result2 = bf.exists('python')
print(bool(result2))  # False  
```

