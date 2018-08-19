关于setting.py的参数



```python

# -*- coding: utf-8 -*-
 
# Scrapy settings for demo1 project
#
# For simplicity, this file contains only settings considered important or
# commonly used. You can find more settings consulting the documentation:
#
#     http://doc.scrapy.org/en/latest/topics/settings.html
#     http://scrapy.readthedocs.org/en/latest/topics/downloader-middleware.html
#     http://scrapy.readthedocs.org/en/latest/topics/spider-middleware.html
 
BOT_NAME = 'demo1'   #Scrapy项目的名字,这将用来构造默认 User-Agent,同时也用来log,当您使用 startproject 命令创建项目时其也被自动赋值。
 
SPIDER_MODULES = ['demo1.spiders']   #Scrapy搜索spider的模块列表 默认: [xxx.spiders]
NEWSPIDER_MODULE = 'demo1.spiders'   #使用 genspider 命令创建新spider的模块。默认: 'xxx.spiders'
 
 
#爬取的默认User-Agent，除非被覆盖
#USER_AGENT = 'demo1 (+http://www.yourdomain.com)'
 
#如果启用,Scrapy将会采用 robots.txt策略
ROBOTSTXT_OBEY = True
 
#Scrapy downloader 并发请求(concurrent requests)的最大值,默认: 16
#CONCURRENT_REQUESTS = 32
 
#为同一网站的请求配置延迟（默认值：0）
# See http://scrapy.readthedocs.org/en/latest/topics/settings.html#download-delay
# See also autothrottle settings and docs  
#DOWNLOAD_DELAY = 3   下载器在下载同一个网站下一个页面前需要等待的时间,该选项可以用来限制爬取速度,减轻服务器压力。同时也支持小数:0.25 以秒为单位
 
    
#下载延迟设置只有一个有效
#CONCURRENT_REQUESTS_PER_DOMAIN = 16   对单个网站进行并发请求的最大值。
#CONCURRENT_REQUESTS_PER_IP = 16       对单个IP进行并发请求的最大值。如果非0,则忽略 CONCURRENT_REQUESTS_PER_DOMAIN 设定,使用该设定。 也就是说,并发限制将针对IP,而不是网站。该设定也影响 DOWNLOAD_DELAY: 如果 CONCURRENT_REQUESTS_PER_IP 非0,下载延迟应用在IP而不是网站上。

#禁用Cookie（默认情况下启用）
#COOKIES_ENABLED = False
 
#禁用Telnet控制台（默认启用）
#TELNETCONSOLE_ENABLED = False 
 
#覆盖默认请求标头：
#DEFAULT_REQUEST_HEADERS = {
#   'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8',
#   'Accept-Language': 'en',
#}
 
#启用或禁用蜘蛛中间件
# See http://scrapy.readthedocs.org/en/latest/topics/spider-middleware.html
#SPIDER_MIDDLEWARES = {
#    'demo1.middlewares.Demo1SpiderMiddleware': 543,
#}
 
#启用或禁用下载器中间件
# See http://scrapy.readthedocs.org/en/latest/topics/downloader-middleware.html
#DOWNLOADER_MIDDLEWARES = {
#    'demo1.middlewares.MyCustomDownloaderMiddleware': 543,
#}
 
#启用或禁用扩展程序
# See http://scrapy.readthedocs.org/en/latest/topics/extensions.html
#EXTENSIONS = {
#    'scrapy.extensions.telnet.TelnetConsole': None,
#}
 
#配置项目管道
# See http://scrapy.readthedocs.org/en/latest/topics/item-pipeline.html
#ITEM_PIPELINES = {
#    'demo1.pipelines.Demo1Pipeline': 300,
#}
 
#启用和配置AutoThrottle扩展（默认情况下禁用）
# See http://doc.scrapy.org/en/latest/topics/autothrottle.html
#AUTOTHROTTLE_ENABLED = True
 
#初始下载延迟
#AUTOTHROTTLE_START_DELAY = 5
 
#在高延迟的情况下设置的最大下载延迟
#AUTOTHROTTLE_MAX_DELAY = 60
 
 
#Scrapy请求的平均数量应该并行发送每个远程服务器
#AUTOTHROTTLE_TARGET_CONCURRENCY = 1.0
 
#启用显示所收到的每个响应的调节统计信息：
#AUTOTHROTTLE_DEBUG = False
 
#启用和配置HTTP缓存（默认情况下禁用）
# See http://scrapy.readthedocs.org/en/latest/topics/downloader-middleware.html#httpcache-middleware-settings
#HTTPCACHE_ENABLED = True
#HTTPCACHE_EXPIRATION_SECS = 0
#HTTPCACHE_DIR = 'httpcache'
#HTTPCACHE_IGNORE_HTTP_CODES = []
#HTTPCACHE_STORAGE = 'scrapy.extensions.httpcache.FilesystemCacheStorage'
 

```

