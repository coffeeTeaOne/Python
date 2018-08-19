### scrapy-CSS选择器





##### 等到一个标签的列表

-   `response.css('title').extract()` 提取对应的标签， 可以接 html 的标签， 如： title、body、div等，也可以是你自定义的 class 标签，

##### 得到第一个标签

-   `response.css('title').extract()[0]`
-   `response.css('title').extract_first()`

##### 获取标签的内容

-   `response.css('title::text').extract_first()`

