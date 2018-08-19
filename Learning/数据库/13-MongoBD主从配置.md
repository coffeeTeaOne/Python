MongoBD主从配置

- 可以在 slave 上启动多个 salve 创建不同的目录进行保存数据，实现master执行20%的写和salve实现80%的查询操作

```python
# master 启动
mongod --bind_ip_all --dbpath /data/master -master

# slave 启动
mongod --dbpath /data/slave -slave -source masterIP:port  # master的IP和端口
```

