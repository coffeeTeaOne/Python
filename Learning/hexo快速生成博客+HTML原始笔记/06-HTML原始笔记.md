---
title: 06-HTML的笔记,还在整理中
date: 2018-03-05 22:10:24
tags: HTML
---

<h4 style="color: #228B22;">笔记还没有整理完,比较乱,凑合着先看吧...</h4>



## HTML 5

```css
超文本标记语言  Hyper Text Markup  Language

 标签  - Tag:      装内容

* 层叠样式表       CSS        渲染引擎  渲染页面

* 脚本语言           JavaScript     执行引擎    进行交互操作  进行触发页面动作

浏览器  Browser   HTML Interpreter

HTTP 传输协议      Hyper Text Transfer Protocol

faststorecapture

```



```python
安装插件代码 Emment
import urllib.request,os; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); open(os.path.join(ipp, pf), 'wb').write(urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ','%20')).read())
```

## 一. 快捷键

```html
! + tab   标签
ctrl + /  注释
ol>li*5>{内容}  快速生成列表   无序 ul  有序ol
ol>li*5>{item $}   有序列表  快捷生成
table>tr*3>td*4  表格  3 行 4 列 
 

HTML5  高级记事本  Atom  TextMate   Vscode
继承开发环境   HBuider
```

**中文乱码**

```html
<head>
	<meta charset="utf-8">   /*标签里没装东西不用写结束框*/
</head>
```

## 二. 常用标签

```css
文本: h1-h6  p  hr br sup sub  em strong
列表:  ul/li    ol/li  dl/dt/dd
链接:
	页面链接
	锚点链接
	功能性链接

图像:img(src  alt)
表格: table/tr/td/th   caption (rowspan/colspan)
音频:audio/source   controls   autoplay  loop
视频:vido/source
网页: a herf    锚点链接  href= "#id"  回到 该id所在的位置

区域: div 块级
      span  行级
<span> 用于对文档中的行内元素进行组合。

<span> 标签没有固定的格式表现。当对它应用样式时，它才会产生视觉上的变化。如果不对 <span> 应用样式，那么 <span> 元素中的文本与其他文本不会任何视觉上的差异。

<span> 标签提供了一种将文本的一部分或者文档的一部分独立出来的方式。
块  变 行级     

表单; form 
action      method="post"  enctype
输入框  label   input/type="text" name=""
				密码  type=password
placeholder  占位提示符
单选框  type=radio  name相同
多选 checkbox
图片  type="file"   multiple 多选
下拉列表  select>option*5   selected默认选中项目  value 是发送到服务器的值  name是给服务器的名字

文本区   texterea rows cols 
生日  日历控件  type="date"
提交   submit
重填  reset

框  fieldset   maxlength 最大长度
   legend   标题
插入网页 iframe  src  frameborde="0"

texterea  文本框
 input  type=range 滑动条  value 是滑动条的最大值
```

### 2.1 常用标签示例

```html
<!doctype html>
<html lang="en">
    <head>
 
        <meta charset="utf-8">
        <title>页面名称</title>
        
    </head>
    <body>
        <h1>
            一级标题
        </h1>
        <h2 id='top'>  id 设置页面其他地方可以快速返回的标记
            二级标题
        </h2>
        <hr> 水平标尺  水平线
        aaaaa<br>    <!--折行标记  换行-->
        <p>
            段落标签  会有一行的空行
        </p>
        <div>
            效果和<br> 相同 换行
        </div>
        <strong>突出显示</strong>
        <em>斜体显示</em>
        <ul>
            无序列表
            <li>列表标签</li>
        </ul>
        <ol>
            有序列表
            <li>列表标签, 会自动写出序号1, 2, ,3...</li>
        </ol>
        <d1>定义列表
        	<dt>定义列表的名称</dt>
            <dd>定义列表的内容</dd>
        
        </d1>
        <a herf="网址">打开网址 所连接的地址</a>
        <a herf='#标记位置'>快速回到本页面的标记的位置, 也可以回到其他页面标记的位置</a>
        <a herf="mailto:邮箱地址,打开发邮件的功能">联系某某,发送邮件</a>
        <a target="_blank" herf="http://wpa.qq.com/......."></a>
        
        **加载图片**
        <img src="图片地址或文件目录" width="图片大小" alt="图片加载失败时会显示的名称">
        
        
   ~~加载表格~~
        <!-- table>tr*3>td*4  三行四列   border 加边框 -->
		<!-- th  加粗显示  corlspan="2"   居中 align="center" -->
		<!-- colspan  跨列   rowspan 跨行 -->
		<!-- 标题  caption -->
		<p>换行</p>
		<table border='1' align="center">
			<caption>表名称</caption>   
			<tr>
				<td align="center">
					<img src="https://www.baidu.com/img/bd_logo1.png" width="50"><br>百度
					<div>百度标签</div>
					<p>标签</p>


				</td>
				<th>2</th>    
				<td>3</td>
				<td>4</td>
			</tr>
			<tr>
				<td>a</td>
				<td>s</td>
				<td colspan="2" align="center">合并</td>
			</tr>
			<tr align="center">
				<td>z</td>
				<td>x</td>
				<td>c</td>
				<td>v</td>
			</tr>
		</table>

		<!-- 播放声音视屏   -->
		<!-- audio 标签 播放声音 -->
		<audio>
			<!-- controls  播放控件  autoplay 自动播放  loop 循环播放 -->
			<audio controls autoplay loop>
			<source src="http://10.7.152.68/%e5%ae%8b%e5%86%ac%e9%87%8e%20-%20%e5%ae%89%e5%92%8c%e6%a1%a5.mp3" >
		</audio>
		<!-- 视频 -->
		<video>
			<video controls>
				<source src="">

		</video>
    </body>
</html>


功能链接  我的QQ链接地址
<a target="_blank" href="http://wpa.qq.com/msgrd?v=3&uin=&site=qq&menu=yes"><img border="0" src="http://wpa.qq.com/pa?p=2:QQ号码:53" alt="张明禄的QQ" title="张明禄的QQ"/></a>
```

**存图片**      文件夹为 imag  imags  images

**声音**        music  文件夹 文件名尽量没有有中文

**SEO**          搜索引擎优化  Search Engine Optimization

​                 网站排名优化   有一个优质的入链接   别人友情链接到自己的网站

### 2.2标签示例

```html
<html>
    <head>
        关键词  网站描述
    </head>
    <body>
        
    </body>
</html>

<!--  代码    示例   -->
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>练习1</title>
	<h1>HELO</h1>
	<h1>你好</h1>

</head>
<body>
	<!--  字符实体替换符-->
	<h2>&copy;&reg;&yen;&amp;&amp;</h2>
	<h1>hel&it;&it;lo,wor&gt;&gt;l&nbsp;&nbsp;&nbsp;d</h1>

	<!-- 嵌入别人的网站  internal fram-->
	<!-- framborder 去边框 -->
	<iframe src="http://www.baidu.com/" frameborder="0" width="300" height="200"> 百度一下</iframe>
		




	<!-- 表单   action -->
	<!--  method  提交表单的方式 get(从服务器拿)  post(提交给服务器) -->
	<!-- 上传表单附件需要用 enctype   readonly 只读不能输入 只能读-->
	<form action="" method="post" enctype="multipart/form-data">
		<fieldset >
			<legend>必填信息</legend>
						<p><!-- 标签 -->
						<label>用户名:</label>
						<!-- name 以后服务器需要拿数据时需要用  name -->
						<!-- placeholder  占位提示 -->
						<input type='text' name='uid' required maxlength="10" placeholder="请输入用户名"></p>
						<p><label>密码:</label>
						<input type="password" name="pwd" placeholder="请输入密码"></p>
						<p><label>确认密码:</label>
						<input type="password" name="repwd" placeholder="请再次输入密码"></p>
						<!-- 开关   单选 -->
						<p>
							<!-- 默认  checked -->
							<label>性别:</label>
							<input type='radio' name="sex" checked>男
							<input type='radio' name="sex" checked>女
						</p>
						<p>
							<!-- 多选  type= checkbox     disabled 不可取消选中-->
							<label>爱好:</label>
							<input type="checkbox" name="fav" checked disabled>游戏
							<input type="checkbox" name="fav">游戏
							<input type="checkbox" name="fav">游戏
							<input type="checkbox" name="fav">游戏
							<input type="checkbox" name="fav">游戏
						</p>
						<p>
							<label >
								头像:
							</label>
							<!-- multiple  表示多选  -->
							<input type="file" name="photo" multiple>

						</p>
						<p>
							<label for="">生日:</label>
							<input type="date" name="birth">


						</p>
						<p>
							<!-- mail 可以提示邮箱格式 -->
							<label for="">邮箱:</label>
							<input type="email" name="mail">


						</p>
						<p>
							<label>籍贯:</label>
							<!-- 下拉列表  select>option*5   value= 后面对应的值提交给服务器的值-->
							<select name="" id="">
								<option value="">北京</option>
								<option value="">上海</option>
								<option value="">广州</option>
								<option value="">甘肃</option>
								<!-- selected 默认选中 -->
								<option value="" selected>四川</option>
							</select>
						</p>
		</fieldset>
		<fieldset>
			<legend>选填信息</legend>
						<p>
							<!-- 文本区 -->
							<label for="">自我介绍</label>
							<textarea rows="5" cols="29"></textarea>
						</p>
						<p>
							<!-- 提交文本 -->
							<input type="submit" value="注册">
							<input type="reset"  value="重填">

						</p>
	</fieldset>

	</form>
</body>
</html>
```

## 三. 样式表

### 3.1优先级

/*id 选择器 > 类选择器 > 标签选择器  > 通配符选择器 */

冲突样式:

 就近原则:        离得近的优先级高

 具体性原则:   那个写的更具体那个生效     写的更加具体的会生效

 重要性原则    !important

### 3.2 背景

*background: 背景色  背景图  平铺选择  偏移*

```css
				background-image:url(img/pic.jpg);
				background-repeat:no-repead; /*平铺*/
				/*background-position: -55px;*/
```

### 3.3 通配符

```css
			*{
				margin:0;    /*元素与元素之间的边距*/
				padding:0;  /*内边距     元素框 与内部元素的边框*/
				font-family:Arial,"幼圆";
				}
```



```css
标签选择器:  
        h2{
			color:red;
        }

#  id 选择  id=""
. 类选择 class=""


div 间距设置: # login div{}   login 下面所有的div 都会被修改
   			# login>div  值对login 下的第一个div 生效
		
按钮:  

盒子模型: 每个元素都是 个盒子
<-- 
    display:none
```

## 四  样式表 要点

## (字体.文本.盒子.列表)

```css
字体
font-size
font-family
font-style
font-weight

文本 
color
text-align
letter-spacing
text-decoration

盒子
padding 内边
border
background    -image   -color 
margin  外边
display
visibility

列表
list-style: squar(圆圈 方块 或者自定义图片)
list-style-poisition: inside/outside;
border-collapse

padding-top:50px;   块内上部间距 50px
text-align: center; 文本居中
text-decoration:underline;   文本装饰  加下划线  none
background-color:pink;   背景颜色
background:  背景色  背景图片  平铺选择  偏移选择
margin: 上 右 下 左;   块外所留的大小
border:none; 去边框
cursor:pointer: 鼠标指针变化
:havor   鼠标悬停变化
:active  激活 点击变色 也可display  

# login input[type="text"], #login input[type="password"] {
border:none;
border-collapse:collapse; 表格合并边框
border-bottom:1px solid green;
outline: none;   输入框边线取消
}


font: font_style size font_family;
background:bk-color  bk-image  bk-repead/no-repead -position 10px;

display:none;   隐藏  所占的位置也消失
visibility:hiden;  隐藏   但占据的空间任然在


伪类 ----

并列 标签  , 隔开  
父子选择     #类/id  > 标签
```

#### 课堂 引入其他字体

```css
font: bold 22px/30px   字体大小/行高
网页经典宽度  960px
适应屏幕尺寸  相对值  

/*传输别人没有的字体*/
css文件加载 乱码    加一行  @charset "utf-8"
引用css文件   link   (不用style)

@font-face {
	font-family: 命名字体的别名; /* 在引用字体格式时可以 引用别名   最好使用期自身的名字*/
	src: url();/* ./ 表示当前路径     将字体文件放在指定的文件夹 添加字体路径*/
}

```

------

宋  衬线字体   笔画粗细不同   边角修饰

非衬线字体       Arial    幼圆 微软雅黑  

等宽字体    每个字母的宽度相同  Curial  



**代码示例**

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title>页面标题</title>
        /*样式表*/
        	/*就近原则  具体性原则*/
			/*id 选择器 > 类选择器 > 标签选择器  > 通配符选择器 */
			/* !important > # > . > h1*/
         <style>
             
             /*标签选择*/
             h2{
                 color:red;
                 font:italic 30px Arial,"幼圆"
                 text-align:center;
                 font-weight:200;
             }
             
             /* id 选择器*/
             #login{
                 width:200px;
                 height:100px;
                 border:3px dotted blue;
                 border-radius:4px 4px 4px 4px;
                 
             }
             
             /*类选择*/
             .foo{
                 display:inline-block;
                 width:50px;
                 text-align:right
             }
             
             .center>input{
                 display:inline-block;
                 width:60px;
                 height:40px;
                 backgrounf-color:blue;
                 font:10px "幼圆";
                 border:none;
                 cursor:pointer;
             }
             
             .center>input:hover{
                 color:red;
             }
             .last{
                 margin-top:30px !important;
                 margin-right:20px !important;
                 text-align:right;
             }
             
             /*激活  悬停*/
             a:active{
                 color:orange;
             }
        </style>
	</head>
	<body>
		<h2>
            VIP用户登陆
        </h2>
        <hr>
        <form id="login" action="" method="post">
            <div>
                <label class="foo">用户名:</label>
                <input type="text" name="uid" id='uid'>
                
            </div>
            <div>
                <label class="foo">密码:</label>
                <input type="pasword" name="psw" id="psw">
                
            </div>
            <div class="center">
                <input type="submit" value="提交">
                <input type="reset" value="重填">
            </div>
            <div class="last">
                <a herf="http://www.baidu.com">新用户注册</a>
            </div>
        </form>
	</body>
</html>

```

### 外部样式表

将样式表放在一个单独的 css 文件中,让其他的网页共用

所有页面可以共享一个样式表

内容与显示分离   方便跟换排版 (皮肤)

```css
引用外部样式表    不用写style  直接link

<link rel="stylesheet" type="text/css" href="css/hwk001.css"/>


```

#### 3.1 下拉菜单 制作

overflow: hidden  ;      将溢出 的内容隐藏起来 在行高足够时将它显示出来

-子框高必须与背景相同  font:    font-type     font-size/fond-mod-size   font-style

高度设置为自动 auto  否则无法显示超出边框的内容



```scss
<style>
			* {
				margin: 0;
				padding: 0;
				/*border:1px solid red;*/
			}
			#Menu li {       /* 设置下面所有行的属性*/
				cursor:pointer;
				width: 150px;
				height: 35px;
				font: bold 22px/35px Arial, '幼圆';  /*行高和字的行高相同*/
				background-color: mediumslateblue;   /*背景色*/
				color: blanchedalmond;       /*前景色*/
				border-bottom: 1px white solid;    /* 也可用margin: 加外边距 */
				overflow: hidden;    /*隐藏*/
				text-align: center;
			}
			#Menu>li:hover{       /*设置主菜单在鼠标触发时的属性*/
				color: darkmagenta;
				height: auto;
			}
			
			#Menu>li>ul>li{
				font: 15px Arial;
				background-color: darkblue;
			}
			a{
				text-decoration: none;
			}
			#Menu a{
				color: white;

			}
			body{
				/*border: 1px solid red;*/
				width: 80%;
				margin:0 auto;
			}
			#Menu{
				width:150px;
				margin: 0 auto;
				
			}
			/*#Menu>li>ul>li{
				margin: 0 0 0 30px ;
			}*/
		</style>

```

#### 4.2 浮动和定位

```css
浮动  folat:right/left

清除浮动  clear:   使后面的内容脱离文档流
/* right   left   both*/
#foo{
				
	clear:right;
			}


定位:
	不适合用margin   padding


相对定位;  poisition : relative  
		 没有脱离文档流 对兄弟元素没有影响  相对于元素原来的位置进行定位
/*块级元素 (block)   行级元素(内联元素   inline)*/
				/*static  正常文档流  (默认模式)   folat */
				/* relative 相对定位    相对原来的位置偏离位置   没有脱离文档流*/
abslout  绝对定位:
				相对于父元素来说设定位置  脱离的正常的文档流
				对兄弟元素有影响   (相当于其不存在的情况下其兄弟元素进行补位了)

fixed  固定定位:
			相对于浏览器窗口来设定元素原来的位置  脱离了文档流


Z-index: num;   数字越大 最后出现   数字小的会被大的覆盖

```

#### 4.3 css hack

```css
css hack
塌陷   	
	<div style="clear: both;"></div>  空白 div 用于方便计算 div高度

ovreflow: auto;    设置自动溢出文档
#foo {    /* 在设置最外层块时添加 overflow:auto; */
				width: 800px;
				border: 1px solid red;
				margin: 0 auto;
				font: 22px "微软雅黑";
				overflow: auto; /* css hack 使外框显示出来*/
			}
```

## 五.   JS  (JavaScript)

```css
JavaScripts:
			behavior   管理页面的行为
            
ECMScripts  -  语法规范 -  主要使用ES 5.1   (最新版 8)
BOM(Browser Object Moder):
    浏览器对象模型- widow (窗口对象模型)
DOM(Document Object Model):
    文档对象模型-Document(文档对象模型) 
    
 	编程范式(理念)
	-面向对象编程
	-函数式编程

js 一般写在 body 最后
		一般是 样式表  css 前置    js 后置
```

### 5.1 简记

null  

IPython

Jupeter  (Python开发)

搜索提示   Ajax    局部刷新

外部引用   <script  scr=""></script>

Ctrl + shift + R  复制代码

### 断点检查  : 浏览器检查

sources  断点检查  onestep by onestep (same with Python)

```javascript
var + 变量名    定义变量
null  空
window.alert('')  弹出显示框
window.promote('')  输入值
window.confirm()  确认选择
if + 条件 {  } else {  }
&&  and 
||   or

弱类型语言:(JS Python)
		变量的类型随它 的赋值而改变
        
定义函数 :
		function foo() {  函数 主体  }    可以 $ 开头   Python 不可以使用$

        \u263a  笑脸

textcontent:
			只显示文本内容 不会显示标签
       
innerHTML:   可显示标签

 直接显示在按钮中 改变按钮的内容
<button onclick="this.innerHTML=Date()">现在的时间是?</button>

=== 为绝对相等，即数据类型与值都必须相等。

条件运算:
	如果变量 age 中的值小于 18，则向变量 voteable 赋值 "年龄太小"，否则赋值 "年龄已达到"。
		voteable=(age<18)?"年龄太小":"年龄已达到";

弹出的输入框:  prompt(' 输入值')
长符计算    .length
查下标      .charAt(index)  //查找指定索引处的对应的参数
转换为number  parseInt(string)
new Date()  创建新对象 日期对象
alert('弹出警告框')

NaN --> Not a Number   不是数字 
isNaN(value) --> 判断 value 不是数字  
undefind --> 未定义
null 空   
'null' --> 字符串空    不输入内容时得到的是
parseInt() 筛选出数字并将其准化为Int类型

parseFloat()

// 全局函数 isNaN 判断是不是 不是一个数    parseInt()  取整数  返回NaN

			// parseFloat()  取小数     只有数字

			// isFinit()  是不是有限的数

			// eval()  计算字符串表达式的值   评估表达式的类型 然后返回评估完后的类型

			// XSS(跨站脚本攻击)

			// 百分号编码,   百分号解码 encodeURIComponent  decodeURIComponent

			var f = encodeURIComponent('张明禄' ,'utf-8')

			alert(f)

```

#### 5.1-1  短路运算--适配低版本浏览器

```javascript
evt = evt || window.event;   //解决兼容问题  
var target = target || srcElement;

bind($('ok'), 'click', foo);
			bind($('ok'), 'click', function(evt) {   //加参数  evt 
				
				// 适配低版本 浏览器      事件绑定为window.event   短路运算
				evt = evt || window.event;
				var target = evt.target || evt.srcElement;//取事件源  解决浏览器兼容问题
				
				if(window.confirm('Are you sure?')) {
					unbind(this, 'click', foo);  // this 当前的$('id')->会和面向对象中的this冲突
					                             //evt.target 也可以拿到事件源 $('ok')
				}
			});
```

#### 5.1-2  绑定事件

```javascript
//更改为简短 的名称
function $(id) {
	return document.getElementById(id)

// 绑定事件
function bind(elem, en, fn) {
	if(elem.addEventListener) {
		elem.addEventListener(en, fn);
	} else {
		elem.attachEvent('on' + en, fn); //ie 浏览器兼容问题
	}
}

function unbind(elem, en, fn) {
	if(elem.removeEventListener) {
		elem.removeEventListener(en, fn);
	} else {
		elem.detachEvent('on' + en, fn);  //ie浏览器兼容问题
	}
}
    

```



### 5. 2 数据类型&运算

```css
简单数据类型:
            number 
            string
            boolean
            null
		   undefind   未定义类型
复杂数据类型:
            object  
typeof 可以开查看数据类型

算数运算符:
		+ - * / % 
赋值运算符:
		=  +=  -=  *= /=  %=
关系运算符:  产生布尔值
		> >= <  =< ==  !=  === (严格等于)     !==(严格不等 )
         1 == "1"   true   js 中有隐式的类型转换  
          ===  三个等号  不带类型准换的比较 

逻辑运算:
		&& (and)   || (or)  ! (变反)
		短路运算  && ||  只要第一个条件成立不会再开其他的条件

自增自减运算符:
			++   --
```

### 5. 3 分支|循环 结构

```css
即使调用函数 
或者使用加号   +  去掉大括号
(function () {
				//循环    for in    for     while  do....while
				var len = carNo.length;   //长度
				for (var i = len - 1; i >= 0; i -= -1) {      // 循环初始条件;控制条件 ; 步进条件 (每循环一次执行都会执行)    条件
					
				}
			}())
			
```

### 5.5 常见事件

```css
事件	描述
onchange	HTML 元素改变
onclick	用户点击 HTML 元素
onmouseover	用户在一个HTML元素上移动鼠标
onmouseout	用户从一个HTML元素上移开鼠标
onkeydown	用户按下键盘按键
onload	浏览器已完成页面的加载
```

### 5.6 JS 代码

```javascript
if (name && )
    //定义变量 var 
			var name = window.prompt('输入名字!')
			//在控制台生成日志  保留
			console.log(typeof(name))
			if(name && name != 'null') {
				window.alert('你好' + name + '!')
			} else {
				window.alert('名字呢?')
				window.alert('你好!');
			}
```

### 5.7 写入内容  innerHTML/write

```javascript
显示日期:
function displayDate(){
	documen.getElementById('demo').innerHTML=Date();
}

使用 window.alert() 弹出警告框。
使用 document.write() 方法将内容写到 HTML 文档中。
使用 innerHTML 写入到 HTML 元素。
使用 console.log() 写入到浏览器的控制台

document.getElementById("demo").innerHTML="段落已修。";  //修改 id 标记处的内容

document.write(Date());  //写入时间
请使用 document.write() 仅仅向文档输出写内容。 //向指定的id处写入内容 覆盖原内容
如果在文档已完成加载后执行 document.write，整个 HTML 页面将被覆盖。

// 变量与函数出现
变量和参数必须以一致的顺序出现。第一个变量就是第一个被传递的参数的给定的值，以此类推
```

#### click事件

```javascript
	<body>
		<p id='demo'> 你好</p>
		<button onclick='foo()'>点击改变</button>
		
		<script>
			//var a = document.write('<h1>标题</h1>')
			function foo(){
				x = document.getElementById('demo')
				x.innerHTML = 'HELLO'
			}
		</script>
	</body>

```

### 5.8 变量赋值  格式化输出

```javascript
var person = {firstName:"John", lastName:"Doe", age:50, eyeColor:"blue"};
document.getElementById("demo").innerHTML =
	person.firstName + " 现在 " + person.age + " 岁.";
```

### 5.8 全局变量  window变量

```javascript
<p>
在 HTML 中, 所有全局变量都会成为 window 变量。
</p>
<p id="demo"></p>
<script>
myFunction();
document.getElementById("demo").innerHTML =
	"我可以显示 " + window.carName;
function myFunction() 
{
    carName = "Volvo";
}
</script>
```

### 5.9 转义字符 显示符号

```javascript
<script>
var x = 'It\'s alright';
var y = "He is called \"Johnny\"";
document.getElementById("demo").innerHTML = x + "<br>" + y; 
</script>
```

### 5.9 switch选择

```css
function myFunction(){
	var x;
	var d=new Date().getDay();
	switch (d){
  		case 0:x="今天是星期日";
    	break;
 		case 1:x="今天是星期一";
        break;
  		case 2:x="今天是星期二";
        break;
        case 3:x="今天是星期三";
   	 	break;
  		case 5:x="今天是星期五";
        break;
  		case 6:x="今天是星期六";
    	break;
		default:
			x = '33333'
 	}
    document.getElementById('demo').innerHTML = x;
```

### 5.10 函数  递归

```javascript
// 1 2 3 个台阶
		// 10 个台阶  有多少种走法
		//卡特兰数
		//迷宫寻路
		//全排列  'ABCDE'


function foo() { //覆盖前面的同名函数
				var sum = 0;
				// arguments 传入的参数组成的一个 类似列表的对象 
				for (var i = 0; i < arguments.length; i += 1) {
					sum += arguments[i];
				}
				return sum;
			}

//递归
	//  递归定义  n! = n * (n-1)!  两边都有相同的方法
			//递归必须有收敛条件
			// 递归公式 
			function f(n) {
				if(n == 1 || n == 0) {
					return 1;
				}
				return n * f(n - 1);
			}

//arguments.callee()  代表正在被调用的函数
return n * arguments.callee(n - 1)

	
		
		<script>	
			(function () {
				function f(n) {
					if (n < 0) {
						return 0;
					} else if (n == 1) {
						return 1;
					}
					return arguments.callee(n - 1) + arguments.callee(n - 2) + arguments.callee(n - 3);
				}	
			} ())

		</script>
```

### 5.11 对象

```javascript
<script>
			var hotel = {
				'room' : 100,
				'name' : '7Days',
				'state' : false,
				'open' : function(){
					this.state = true;
					alert('开始营业')
				},
				'book' : function (num) {
					this.room -= num;
					alert(hotel.room);
				},
				
			}
			//alert(hotel.name)
			//alert(hotel.state)
			hotel.open()
			alert(hotel.state)
			hotel.book(4)	
		</script>

```

### 5.12 bind / this与面向对象里面冲突

```javascript
// this 冲突
<script>
			var hotel = {
				'room': 100,
				'name': '7Days',
				'book': function(num) {
					if(this.room - num > 0) {
						this.room -= num;
						alert(hotel.room);
					} else {
						alert('订房失败, 剩余' + this.room + '间房间!')
					}
				}
			};
			hotel.open()
			bind($('B'), 'click', function() {   //this冲突
				var num = parseInt($('numB').value);
				hotel.book(num);
			});
		</script>

/////////////////////////////////////////////////////////////////////////////////
			function foo() {
				alert('你好')
			}

			bind($('ok'), 'click', foo);
			bind($('ok'), 'click', function(evt) {
				evt = evt || window.event;
				var target = evt.target || evt.srcElement;
				if(confirm('确定吗?')) {    //里面的this  会和JSON对象中this的内容冲突
					unbind($('ok') / this / evt.target, 'click', foo)  
//					$('ok').removeEventListener('click', foo)
				}
			})
		</script>

//  src function  ///////////////////////////////////
function $(id) {
	return document.getElementById(id)
}

function bind(elem, en, fn) {
	if(elem.addEventListener) {
		elem.addEventListener(en, fn);
	} else {
		elem.attachEvent('on' + en, fn); //ie 浏览器
	}
}

function unbind(elem, en, fn) {
	if(elem.removeEventListener) {
		elem.removeEventListener(en, fn);
	} else {
		elem.detachEvent('on' + en, fn);  //ie浏览器
	}
}
```

## 博客

github.com

coding.net  // 私有

gitee.com

### 5.13-浏览器使用

#### 1. woidow

- location  href/reload() /replace() 
- history   go()  / forward()   / back()
- navigator
- screen  availWidth/  availHeight
- alert()   promot()  confirm()
- setTimeout   setInterval()
- clearTimeout   clearInterval
- moveTo()  /   By()  resizeTo()  /By()    ---不常用



```javascript
BOM
Browser Object Model 

对象
window对象
window.innerwidth  窗口宽
window.innerheight
document
- setTimeout()  到指定时间执行操作
setInterval()
getFullYear() 获得完整的年份  getYear() 得到的是距离1900多少年
var date = new Date()  创建对象 要用关键字 new

三元条件运算
timeStr += hour < 10 ? '0' : ''  
 如果后面的条件成立 则 + 0 不成立 + ''
Python   return  a if a > 0 else 0

创建新的对象

var date = new Date();

// example
(function () {
				showTime();
				var timer = 0;
				$('ok').addEventListener('click', function (evt) {
					if (timer != 0) {
						window.clearInterval(timer);
						timer = 0;
						evt.target.textContent = '继续';
					} else {
						timer = window.setInterval(showTime, 1000);
						evt.target.textContent = '暂停';
					}
				});
				timer = window.setInterval(showTime, 1000);
			
			} ())

$(elem).firstChild.拿elem的第一个子元素

getElementsBytagname   得到的是一个列表  按照标签tag 拿元素 
getElementsByclassname   查下标找出元素 类 拿元素

querySelector 找某个元素   + All 找所有的元素  相当于css中的选择器
querySelector('#adv>img') 

navigator 浏览器
navigator.appName  浏览器名字


方法:
	blur() 失去焦点
    focus() 获得焦点
    
    open()  在当前页面开新窗口  浏览器会默认拦截
	history  访问的历史记录  back()  forward()   go(num) num > 0 前进; num < 0 后退
    
    loaction 地址栏
    location.reload 刷新页面
    location.	
```

#### 2. document

- Document-Object-Model

查找元素方法

- getElementById()
- getElelementsByTagName()
- getElementsByClassName()
- querySelector()     单个
- querySelectorAll()    所有

修改节点内容

- textContent    
- innerHTML    可带标签
- nodeValue

```javascript
取页面任意元素
//id 取元素
document.getElementById()
 
//类名取元素   得一个列表
var elems = document.getElementByClassName('className') 
for (var i = 0; i < elems.length; i += 1) {
	//elem.item[i] 和直接去相同  改变背景颜色
			elems[i].style.backgroundColor = 'coral';
				}

//标签名取元素   得到的也是一个列表 
var elems = document.getElementByTagName('tagName')
for (var i = 0; i < elems.length; i += 1) {
					elems[i].textContent = 'abc'
				}
//获取需要的id 或者 class 下的元素
document.querySelector() // 拿一个元素
var hs = document.querySelectorAll("#page>h2");
			for (var i = 0; i < hs.length; i += 1) {
				hs[i].textContent = 'hello';
			}

//修改节点
-textContent   改文本

-innerHTML    可带标签

-nodeValue    拿节点


//  nodeValue
var hs = document.querySelectorAll("#page>h2");
			for (var i = 0; i < hs.length; i += 1) {
				//文本也是一个节点
				hs[i].firstChild.nodeValue = 'Dinner';
                 hs[i].id = 'foo';  // 添加 id
			    hs[i].setAttribute('id', 'bar');  //设置属性
				hs[i].getAttribute()  //获取属性
				hs[i].removeAttribute('id')  //移除属性
            }

通过访问成员运算符可以访问属性 (.)
removeAttribute()   移除属性

//添加删除节点
			var ul = document.querySelector("#page>ul");
			//ul.innerHTML += '<li>111</li>';
			var li = document.createElement('li');
			li.className = 'hot';
			li.textContent = 'new line';			 
//添加在末尾
			ul.appendChild(li); 
//添加在设置位置
			ul.insertBefore(li, ul.firstChild)
//删除节点
removeChiled()

//如果已经获得一个节点,  如何访问他的父节点,子节点, 兄弟节点
//父节点
parentNode  
childen[0/1/2...]   //子节点
firstChild    lastChild
//兄弟节点
nextSibling  //后面的兄弟
prevSibling  //前面的 兄弟

+ 紧挨着的兄弟   ~ 所有

position:relative ;
left : 30px; 相对于原来的位置偏移
```

## about key

```javascript
封装
 //性能略微优化   cache  缓存  方便获取已经获取过的元素
// 缓存 : 优化一般都是优化时间   牺牲空间来获取时间  
var cache = {};
function $(id) {
	if(!cache[id]) {
		cache[id] = document.getElementById(id);
	}
	return cache[id];
}

读取样式
// IE 兼容   取元素的只读样式
// 三元条件运算     
function getStyle(elem) {
	return window.getComputedStyle ? window.getComputedStyle(elem) : elem.currentStyle
}

var h1Style = window.getComputedStyle($('foo'));
			var h1Size = 4 * parseInt(h1Style.fontSize); //只能拿到数字,单位最后要再添加一次
			$('foo').style.fontSize = h1Size + 'px';

			// 全局函数 isNaN 判断是不是 不是一个数    parseInt()  取整数  返回NaN
			// parseFloat()  取小数     只有数字
			// isFinit()  是不是有限的数
			// eval()  计算字符串表达式的值   评估表达式的类型 然后返回评估完后的类型
			// XSS(跨站脚本攻击)
			// 百分号编码,   百分号解码 encodeURIComponent  decodeURIComponent
			var f = encodeURIComponent('张明禄' ,'utf-8')
			alert(f)
```

### 恶意广告,添加闪烁颜色

```javascript
广告
var counter = 0;
			$('close').addEventListener('click', function() {
				counter += 1;
				if(counter < 3) {
					window.open('http://www.baidu.com')
				} else {
					$('adv').style.display = 'none';
				}

			})

//闪烁
function randomColor() {
				var r = parseInt(Math.random() * 256);
				var g = parseInt(Math.random() * 256);
				var b = parseInt(Math.random() * 256);
				return 'rgb(' + r + ',' + g + ',' + b + ')';
			}

			(function() {
				var counter = 0;
				$('addBtn').addEventListener('click', function() {
					if(counter < 50) {
						counter += 1;
						var div = document.createElement('div');
						div.className = 'piece';
						div.style.backgroundColor = randomColor();
						$('container').insertBefore(div, $('container').firstChild);
						//$('container').appendChild(div);
					}
				});

				var timer = 0;
				$('flaBtn').addEventListener('click', function() {
					if(!timer) {
						timer = window.setInterval(function() {
							var allPieces = $('container').children;
							for(var i = 0; i < allPieces.length; i += 1) {
								allPieces[i].style.backgroundColor = randomColor();
							}
						})

					}
				}, 1000)

			});

			}());
```

#### 抠图

```javascript
background: url(img/ui-icons-trans.png) no-repeat -95px -15px;

//事件回调函数    知道事件发生时会做什么,但不知道事件什么时候发生  
			// evt 是浏览器回调函数时传入的
			function rmItem(evt) {
					var li = evt.target.parentNode; //拿到父节点
					$('fruits').removeChild(li);
				}
			
			$('addBtn').addEventListener('click', function() {
				var fName = $('fInput').value.trim();
				if(fName != '') {
					var li = document.createElement('li');
					var a = document.createElement('a');
					a.href = "javascript:void(0)";
					a.addEventListener('click', rmItem);
					li.appendChild(a);
					$('fruits').appendChild(li);
					$('fInput').value = '';
					$('fruits').focus();
				}
			});
			
			var delBtns = document.querySelectorAll('#fruits a');
			for (var i = 0; i < delBtns.length; i += 1) {
				//  绑定事件   用 evt.target 绑定每个事件  i 在 结束后会消失
				delBtns[i].addEventListener('click',rmItem);
			}
```

## 正则

```javascript
//匹配字符串的匹配模式    正则表达式就是定义字符串的匹配模式的工具

元字符
.  任意字符
/w 
/d
/s  空白字符
/b  单词的开始或者结束
^   整个字符串的开始
$   结束

量词  
*  0次或多次
+  1次或多次
?  0次或一次
{} 

反义  [qwer]   [^qwer] 反义
\W  \D  \S  \B

分支  |

分组 ()

var reg = /^1[3456789]\d{9}$/;   // 验证手机号
var reg = new RegExp('^[3456789]\\d{9}$')
					
```

### 广告

```
左上角坐标  offsetLeft   offsetTop  


```

### moveDiv

```javascript
//移动方块 
<body>
		<div id="one"></div>
		<div id="two"></div>

		<script src="js/func.js"></script>
		<script>
			

			function makeDraggable(elem) {
					bind(elem, 'mousedown', function(evt) {
						evt = evt || window.event;						
						elem.isMouseDown = true;
						var divStyle = getStyle(elem); //得到div的可读样式
						elem.originLeft = parseInt(divStyle.left); //得到div的位置
						elem.originTop = parseInt(divStyle.top);
						elem.mouseX = evt.clientX;
						elem.mouseY = evt.clientY;
					});
					bind(elem, 'mouseup', function() {
						elem.isMouseDown = false;
					});
					bind(elem, 'mousemove', function(evt) {
						if(!elem.isMouseDown) return;
						evt = evt || window.event;
						var dx = evt.clientX - elem.mouseX;
						var dy = evt.clientY - elem.mouseY;
						elem.style.left = (elem.originLeft + dx) + 'px';
						elem.style.top = (elem.originTop + dy) + 'px';
					});
				};
			(function() {				
				makeDraggable($('one'));
				makeDraggable($('two'));
			}());
		</script>
	</body>
```

### 表单验证

```javascript
<script>
			(function() {
				bind($('login'), 'submit', function(evt) {
					evt = evt || window.event;
					prevDefault(evt) // 阻止事件的默认行为, 这里阻止表单的提交
			//验证表单  
			//匹配字符串的匹配模式    正则表达式就是定义字符串的匹配模式的工具
					var telReg = /^1[3456789]\d{9}$/;   // 验证手机号					
					var uidReg = /^[a-zA-Z_]\w{5,19}$/;  // 验证用户名
					var qqReg = /^[1-9]\d{4,}$/            // 验证qq号码
					var userName = $('username').value.trim();
					if (uidReg.test(userName)) {
						
					} else {
						$('uidHint').textContent = '输入错误'
						$('uidHint').classname = 'incorrect'
					}
										
			//验证通过
					var target = evt.target || evt.srcElement;
					target.Submit(); //最后手动提交表单 $('login').submit();
				})
			}());
		</script>
```

### 提交表单

```javascript
/**
 * 阻止事件的默认行为  例如 阻止表单默认的submit行为 使submit失效
 * @param {Object} evt
 */
function prevDefault(evt) {
	if (evt.preventDefault) {
		evt.preventDefault();
	} else {
		evt.returnValue = false;
	}
}

//修改某个 elem 的属性  attribute
elem.style.attr = '' ;  进行修改

//创建元素  
var div = document.creatElement('div');
//添加元素
$('id').appendChild(div);
//每次都插入到其第一个子元素之前
$('id').insertBefore(div, $('container').firstChild);
//设置属性
newDiv.style.backgroundColor = 'red';

```

## 5.14事件冒泡  事件捕获

- 事件捕获   parent-->child 从上层往下层依次执行事件所引发的动作

- 事件冒泡   (默认模式) child-->parent  IE (其他浏览器不用)

- 阻断事件传播

  传入evt 参数 进行 evt.stop.Propagation();阻止冒泡或者捕获的行为  事件到此为止

```javascript
function bind(elem, en, fn) {
	if(elem.addEventListener) {                
		elem.addEventListener(en, fn, true);  //// 第三个参数表示事件处理的方式     默认为 false (bubble)    false 事件冒泡(bubble), true 事件捕获(capture)
	} else {
		elem.attachEvent('on' + en, fn);
	}
}
				bind($('three'), 'click', function (evt) {
					evt = evt || event;
					alert('i am three');
					//阻止事件传播(阻止冒泡行为)      	evt.stopPropagation
					//事件捕获还是能正常进行   第三个参数是true时不会阻止
					if (evt.stopPropagation) {
						evt.stopPropagation();
					} else {
						evt.cancelBubble = true;
					}
					
				});

/**
 * 处理事件对象 解决兼容性问题
 * @param {Object} evt
 */
function handleEvent(evt) {
	evt = evt || event;
	evt.preventDefault = evt.preventDefault || function () {
		//没有属性绑定函数
		this.returnValue = false;
	};
	evt.stopPropagation = evt.stopPropagation || function () {
		this.cancelBubble = false;
	};
	return evt;

    //例子   
bind($('three'), 'click', function (evt) {								
					evt = handleEvent(evt);
					evt.stopPropagation();
					
```

### evt参数

```javascript
evt 参数代表的是事件对象  绑定了和事件有关的所有信息
如果事件回调函数中药用到事件相关的属性和方法 最好就是指定 evt 参数
 target / clientX button / keyCode   
 preventDefault() / stopPropagation()   
 不管函数是否指定了 evt 参数 当事件发生回调该函数时都会传入该参数  ()没有可以用arguement
 
 //this   谁调用函数谁就是 事件源 
```



## 六.  JS 库

```javascript
jQuery   javascript Query(查询)      motto (口号): Write Less Do More    
https://cdn.bootcss.com/jquery/3.3.1/jquery.min.js   压缩版本
越高的版本对低版本的IE的兼容性越差  如 1.x  兼容最古老的IE
体积较大  没有模块化  只能全部使用  


下载 网站 bootcss.com 前端开发下载资源


BootStrap  适应屏幕尺寸框架
react  使用 安卓 和 ISO 两套

CDN 服务  (内容分发网络) 找到离的最近的服务器来加载 资源  解决加载慢
connect Delivery Network

```

### jQuery基本命令

```css
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

### css() 和 attr() 区别

```javascript
在jquery里，css和attr都可以动态的修改html中元素的属性。但css()是用来操纵元素style{}的，而attr()是用来操作元素固有的属性的，且attr()的权重比css()要大，它会覆盖css()的样式。例：$("#txt").css("display","none")
$("#txt").css({"display":"none","width":"5px",....})       $("#txt").attr("title","zz")
```



#### 代码示例

```javascript
	$("#poem p").on('click', function () {
				//其他的变蓝
				$("#poem p").css('background-color', 'darkcyan') ;   //两个参数 第一个是属性名  第二个是属性值 
											// 一个参数表示读取样式
				$(this).css('background-color', 'red');  //点中 的变红
			});

/// 链式  开火车式编程
<script>
			//回调函数
			$(function() {
				function removeItem(evt) {
					$(evt.target).parent().remove();
				}
				$("#fruits").on('click', removeItem);

				//选择器
				$('#addBtn').on('click', function() {
					// .val() 不传参  取值  传参  赋值
					var fruitName = $('#fInput').val().trim();
					if(fruitName != '') {
						//创建标签
						var li = $('<li>').text(fruitName);
						//  修改 属性       链式编程   开火车编程   可以对一个 elem 进行多种方法一次顺序操作
						var a = $('<a>').attr('href', 'javascript:void(0)').on('click', removeItem);
						li.append(a);
						$('#fruits').append(li);
						//修改属性      得到的都是数组
						$("#fInput").val('')[0].focus();
					}

				});
			});
</script>
```

## Linux 系统概况

```
redHat
Ubantu
CentOS
SUSE
Sebian
VMWare / VirtualBox 免费

<canvas>  2d 绘图环境  2d 游戏  var ctx = canvas.getContext("2d")

<WebGL>  3d 渲染

pps 幻灯片自动播放

```

## 面向对象使用

```javascript
创建对象
实例化对象  用 new 参数
//构造器函数    创建对象
 Plane.prototype.init = function () {}

			function Plane(x, y, speed) {
				this.x = x;
				this.y = y;
				this.speed = speed;
				this.flag = true;

				this.init = function() {
					this.div = $('<div>').addClass('plane').css('left', this.x + 'px').css('top', this.y + 'px');
					$(document.body).append(this.div);
				};
				this.fly = function() {
					var image = this.flag ? '../img/hero1.png' : "../img/hero2.png";
					this.div.css('background-image', 'url(' + image + ')');
					this.y -= this.speed;
					this.div.css('top', this.y + 'px');
					this.flag = !this.flag;
				};
			};
			$(function () {
				var planes = [];
				for (var i = 0; i < 10; i += 1) {
					var speed = parseInt(Math.random() * 7 + 2);
					var plane = new Plane(100 + 120 * i, 560, speed); // 创建对象用 new
					plane.init();
					planes[i] = plane;
				}
				window.setInterval(function () {
					for (var i = 0; i < planes.length; i += 1) {
						planes[i].fly();
					}
				}, 100);
			})

///////////////////////////////////////////////
//构造器函数    创建对象
			function Plane(x, y, speed) {
				this.x = x;
				this.y = y;
				this.speed = speed;
				//this.jetting = true;
			}
			Plane.foo = function () {
				alert('Hello')
			}
			//绑定原型方法   将不变写在外面   将 变化的 属性写在创建对象里面
			Plane.prototype.jetting = true;

			Plane.prototype.init = function() {
				this.div = $('<div>').addClass('plane').css('left', this.x + 'px').css('top', this.y + 'px');
				$(document.body).append(this.div);
			};
			Plane.prototype.fly = function() {
				var image = this.jetting ? '../img/hero1.png' : "../img/hero2.png";
				this.div.css('background-image', 'url(' + image + ')');
				this.y -= this.speed;
				this.div.css('top', this.y + 'px');
				this.jetting = !this.jetting;
			};
			//数组删除元素 应该从后面往前删除  避免索引的
			Plane.prototype.destroy = function () {
				for (var i = 0, i < palnes.length; i += 1) {
					planes[i].fly();
					if (planes[i].y < -124) {
						palnes[i].destroy();
					}
				}
			}
			$(function() {
				var planes = [];
				for(var i = 0; i < 10; i += 1) {
					var speed = parseInt(Math.random() * 7 + 2);
					var plane = new Plane(100 + 120 * i, 560, speed); // 创建对象用 new
					plane.init();
					planes[i] = plane;
				}
				window.setInterval(function() {
					for(var i = 0; i < planes.length; i += 1) {
						planes[i].fly();
					}
				}, 100);
			})
```

### 删除数组元素

```
应该注意不能从前面删除,会打乱数组的下标   可以从最后一位开始删除

```

## jQuary 插件

```javascript
 UI 插件
jQueryui.org.cn       中文网  可以下载 各种插件
效果   换主题  动画效果
images 放在 css 文件目录下
cdn 服务器 链接 样式表  提供最近的CDN 来进行加载我们需要的样式表
按照其给定的格式进行 排版  
```

## 七. Ajax技术

```javascript 
Asynchronous JavaScript and XML  (异步相应)
www.   World   Wide  Web  万维网
					Wait 
					
异步获取服务器数据   ( 异步 不阻塞 ) (同步  阻塞  排队等待)
等服务器相应数据后 (Json/XML) 就可以对页面进行局部刷新
就可以在不中断用户体验的前提下刷新页面

瀑布式加载

数据 XML 和 JSON(主流)

有时虽然有接口但是服务器不支持 提供数据 js 会拿不到数据 Python 可以拿到
// JavaScript 发送HTTP请求默认只支持同源策略, 只能取自己网站的数据
// 如果要跨域取数据 是需要对方提供数据的服务器是支持的
// 如果对方支持模式   JSONP / 
//  服务器设置 Cross-Origin: * ; 支持跨域取数据

```

### Ajax 技术原生代码

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

### jQuery-Ajax代码

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

## 页面简易写法

```javascript
BootStrap
提供框架 直接在生成代码  修改代码即可
bootcss.com
响应式布局  可以适配各种屏幕尺寸   封装了样式表和 js
有现成的 css 和 js  


bootStrap可视编辑器/layoutit 在线可视化编辑
设置好页面布局 下载代码 粘贴在body里
<script src="js/jquery.min.js"></script>
		<script src="https://cdn.bootcss.com/bootstrap/3.3.7/js/bootstrap.min.js" integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>

```







