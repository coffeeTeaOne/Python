

浏览器对象模型-BOM(Browser Object Model)

文档对象模型-DOM(Document Object Model)

语法规范 ECMScripts - 主要使用 ES 5.1 (最新是ES 8)



### 1.Javascript常用的命令

[示例代码可在这篇笔记中查找, 这里是整理过后的一些总结](https://zhangminglu.github.io/2018/03/05/06-HTML%E5%8E%9F%E5%A7%8B%E7%AC%94%E8%AE%B0/)

- windonw
  - alter / promote / confirm
  - location href / reload() / replace()
  - history go() / forward() / back()
  - setTimeout()/setInterval()
  - clearTiameout()/clearInterval()
- document
  - getElementById() / TagName() / ClassName()
  - querySelector() / querySelectorAll()
  - textContent (文本)/ innerHTML(文本+标签) / nodeValue(节点)
- 图片处理, 抠图 

```javascript
background: url(img/url.jpg) norepeat -95px -15px;位置
```

- 事件冒泡和事件捕获
  - 事件冒泡: child --> parent 从下往上传播 (IE)
  - 事件捕获: parent --> child 从上往下传播
- 阻止事件的传播
  - evt.stop.Propagation(), 事件到此为止, 不再继续传播
- evt参数
  - evt参数表示的是时间对象, 绑定了和事件有关的所有信息, 如果事件回调函数中要用到事件相关的属性和方法, 最好就是指定evt参数
  - this 时间源, 谁调用函数谁就是事件源



**J S 中的三元条件运算**

```javascript
function getStyle(elem) {
	return window.getComputedStyle ? window.getComputedStyle(elem) : elem.currentStyle
}
```



### 2.JQuery框架

- 基本命令

```javascript
在window.jQuery = function () {}绑定
var $ = window.jQuary;
jQuary('#id') = $('#id')
jQuery.noConflict();   //将 $ 函数不用  用 jQuery 代替   解决引入多个库的 $ 冲突,   将原来使用 $ 的地方换为 jQuery

$('#id')  选择elem 转化为 jqelem
jqelem.fadeIn(1000)   淡入
jqelem.fadeOut(1000)   淡出
jqelem.on('click', function () {})   绑定事件
jqelem.attr('class/type/', '设置值');  设置 属性
jqelem.css('', '')  设置 css  style
$('#id').one('click', function () {}) 只点击一次  执行函数
$('#id').each(function (){}) 对选中的div 的每一个执行操作
$("#id").text('abc')  将字符串作为 #id 标签的的内容写入
        .html('') 加标签 
$("id").val()    读取该标签里面的值


// 链式编程开火车式编程    可以对一个 elem 进行多种方法一次顺序操作  元素还是原来的这个元素


事件传播
???????????????????????????????????
jQuary对象
	不需要考虑浏览器兼容性问题
	jQuary 的本质是一个数组 包含着所有的获得的元素
	如果需要将jQuary 还原成原生的JS对象  - 下标运算可以得到   或  get(index)      
$(item)
1. $(function())  绑定该函数是页面加载完成后进行的回调函数
2. $(selector)   传入的参数是一个选择器  通过选择器获得对应的元素,并将其处理成 3. $(elem)   传入的参数是一个原生 JS 对象   event.target / this 
			将原生 JS 对象转变为 jQyary 对象  jQuary 有更多的属性和方法
4. $(tag)  传入的是标签   创建标签对应的元素 并处理成 jQuary 对象


查找元素

- 选择器
  - * / element / #id / .class / selector1, selector2
  - ancestor descendant / parent>child / previous+next / previous~siblings 
- 筛选器
  - 基本筛选器：:not(selector) / :first / :last / :even / :odd / :eq(index) / :gt(index) / :lt(index) / :animated / :focus
  - 内容筛选器：:contains('…') / :empty / :parent / :has(selector)
  - 可见性筛选器：:hidden / :visible
  - 子节点筛选器：:nth-child(expr) / :first-child / :last-child / :only-child
  - 属性筛选器：[attribute] / [attribute='value'] / [attribute!='value'] / [attribute^='value'] / [attribute$='value'] / [attribute|='value'] / [attribute~='value']
- 表单：:input / :text / :password / :radio / :checkbox / :submit / :image / :reset / :button / :file / :selected / :enabled / :disabled / :checked

执行操作

- 内容操作
  - 获取/修改内容：html() / text() / replaceWith() / remove()
  - 获取/设置元素：before() / after() / prepend() / append() / remove() / clone() / unwrap() / detach() / empty() / add()
  - 获取/修改属性：attr() / removeAttr() / addClass() / removeClass() / css()
  - 获取/设置表单值：val()
- 查找操作
  - 查找方法：find() /  parent() / children() / siblings() / next() / nextAll() / prev() / prevAll()
  - 筛选器：filter() / not() / has() / is() / contains()
  - 索引编号：eq()
- 尺寸和位置
  - 尺寸相关：height() / width() / innerHeight() / innerWidth() / outerWidth() / outerHeight()
  - 位置相关：offset() / position() / scrollLeft() / scrollTop()
- 特效和动画
  - 基本动画：show() / hide() / toggle()
  - 消失出现：fadeIn() / fadeOut() / fadeTo() / fadeToggle()
  - 滑动效果：slideDown() / slideUp() / slideToggle()
  - 自定义：delay() / stop() / animate()
- 事件
  - 文档加载：ready() / load()
  - 用户交互：on() / off()
```



- css() 和 attr() 区别

  - 但`css()`是用来操纵元素`style{}`的，而`attr()`是用来操作元素固有的属性的，且attr()的权重比css()要大，它会覆盖css()的样式。

  ```javascript
  $("#txt").css("display","none")
  $("#txt").css({"display":"none","width":"5px",....})   $("#txt").attr("title","zz")
  ```

  

### 3.Ajax异步IO技术

- 异步获取服务器数据   

  - ( 异步 -不阻塞 ) (同步 - 阻塞  排队等待)

- 服务器取得相应数据 (Json/XML) , 就可以对页面进行局部刷新,就可以在不中断用户体验的前提下刷新页面

  - 瀑布式加载

  

  - JavaScript 发送HTTP请求默认只支持同源策略, 只能取自己网站的数据
  - 如果要跨域取数据 是需要对方提供数据的服务器是支持的
  - 如果对方支持模式   JSONP 
  - 服务器设置 Cross-Origin: * ; 支持跨域取数据

**代码示例, 原生, 无JQuary**

```javascript
	<body>
		<div id="mm"></div>
		<button id="loadBtn">加载</button>
		<script src="js/jquery1.12.min.js"></script>
		<script>			
			//JavaScript 发送HTTP请求默认只支持同源策略
			$(function() {
				$('#loadBtn').on('click', function() {
					var url = 'http://api.tianapi.com/meinv/?key=30cb00f0e0f6c2f605ba1ebca41c3282&num=2';
					$.getJSON(url, function(obj) {
						for(var i = 0; i < obj.newslist.length; i += 1) {
							// $ 就是 jQuary 
							$('#mm').append(
								$('<img>').attr('src', obj.newslist[i].picUrl).attr('width', '300')
								);
						};
					});
				});
			});
		</script>
	</body>
```

**jQuery-Ajax代码**

```javascript
1. $.getJSON(url, function() {}) // 请求成功执行function函数

2. $.Ajax({
    'url':'',
    'type': '' ,
    'data': {
        'key': '',
        'num': ''
    },
    'dataType': 'json',  //默认为json  也可以使用 XML 其他格式
    'success': function(){}  //请求成功 进行的函数
    'error': function(){}  //失拜  执行的函数
    
})
Ajax 加载图片  瀑布式

<!DOCTYPE html>
<html>

	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>

	<body>
		<div id="mm"></div>
		<button id="loadBtn">加载</button>

		<script src="js/jquery1.12.min.js"></script>
		<script>
			$(function() {
				$('#loadBtn').on('click', function() {
					var url = 'http://api.tianapi.com/meinv/?key=30cb00f0e0f6c2f605ba1ebca41c3282&num=2';
					$.getJSON(url, function(obj) {   // obj 通过url 是得到的 jQuery对象数组
						for(var i = 0; i < obj.newslist.length; i += 1) {
							$('#mm').append(
								$('<img>').attr('src', obj.newslist[i].picUrl).attr('width', '300')
								);
						};
					});
				});
			});
		</script>
	</body>
</html>
```

##  