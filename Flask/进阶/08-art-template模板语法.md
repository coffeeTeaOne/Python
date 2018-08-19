

### 特性



1.  性能卓越 ,执行速度是 Mustache 和 tmpl 的 20 多倍
2.  支持运行时调试, 精确定位异常语句
3.  对 NodJS Express 友好的支持
4.  安全, 默认对输出进行转义, 在沙箱中运行编译后的代码
5.  支持 include 语句
6.  可在浏览器端实现按路径加载模板
7.  支持编译, 可将模板zhuan换成非常精简的 js 文件
8.  模板语句简洁, 无需前缀引用数据, 有简洁版本与原生语法版本可选
9.  支持所有流行的浏览器



#### 语法

> 在 jinjia2 中使用 {% raw %}  {% endraw %}  将 template 语法隔离开

1.  引用简洁的语法的引擎模板

    -   `<script src='dist/teplate.js'></script>`

2.  表达式   `{{  }}`

3.  输出表达式

    -   对内容进行编码输出  `{{ content }}`
    -   对内容不变吗输出 `{{ #content }}`

    >   编码可以防止数据中含有 HTML 字符串, 
    >
    >   避免引起 XSS 攻击

4.  条件表达式

```javascript

{{ if admin }}
	<p>admin</p>
{{ else if code > 0 }}
	<p>master</>
{{ else }}
 	<p>error!</p>
 {{ /if }}
```

5.  遍历表达式

    -   无论数组或者对象都可以使用 each 进行遍历

    ```javascript

    {{ each list as value index }}
    	<li>{{ index }} - {{ value.user }}</li>
    {{ /each}}
    ```

    简写

    ```javascript

    {{ each list }}
    	<li>{{ $index }} - {{ $value.user }}</li>
    {{ /each }}
    ```

    ​

6.  模板包含表达式

    -   用于嵌入式子模板  `{{ include 'template_name' }}`
    -   子模板默认共享当前数据, 也可以指定数据: `{{ include 'template_name' news_list }}`

7.  辅助方法

    ```javascript

    template.helper('dateFormat', function (date, format) {
        
        return value;
    })

    模板中使用的方式  {{ time|dateFormat: 'yyyy-MM-dd hh:mm:ss' }}

    支持传入参数和嵌套使用: {{ time|say:'cd'|ubb|link }}
    ```

    
