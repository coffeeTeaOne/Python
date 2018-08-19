

#### 1.常用的标签

- 文本

  - h1 - h6  / p / hr / br / sup / sub / em / strong  

- 列表

  - ul-li  / ol-li / dl/ dt / dd      table-th-tr-td-caption(rowspan/colspan)

- 图像

  - image(src='' alt='')

- 音频视屏

  - audio/source  controls  autoplay loop
  - vidieo / source      \<audio controls autoplay loop>

- 区域

  - div  / span

- 表单

  - form   外框-fieldset   表单标题-lengenda  标签label  输入框 input

  - input   文本-text 密码-password 占位提示符-placeholder

    - 单选框 type=radio   多选 checkbox
    - 上传文件  type=file 指定名字name
    - 提交 submit  重填 reset

  - 下拉菜单   select>option*5    selected默认选中, value发送到服务器的值name是发送的名字

  - 文本

  - textarea   rows/cols     日历 type='data'

    

#### 2.层叠样式表

- 优先级排序
  - id选择器(#) > 类选择器(.) > 标签选择器 (div)> 通配符选择器(*)
- 样式冲突
  - 就近原则: 离得近的优先级高
  - 具体性原则: 选择的器写的更加具体优先
  - 重要性原则: `!important`在标签后加



**关键词组命令**

- background: 背景色 背景图 平铺选择(repead) 偏移
- 字体
  - font-size   font-family  font-style font-weight
- 文本
  - color  text-align letter-spacing  text-decorator
- 盒子
  - padding margin border background display visibility
- 列表
  - list-style(列表前的小圆点的样式)  list-style-position  border-collapse

```css
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
```

- 在css中使用自定义字体

```css
/*传输别人没有的字体*/
css文件加载 乱码    加一行  @charset "utf-8"
引用css文件   link   (不用style)

@font-face {
	font-family: 命名字体的别名; /* 在引用字体格式时可以 引用别名   最好使用期自身的名字*/
	src: url();/* ./ 表示当前路径     将字体文件放在指定的文件夹 添加字体路径*/
}

```



- 浮动和定位
  - float 浮动, 清除浮动, clear

```css
浮动  folat:right/left
定位:不适合用margin   padding
清除浮动  clear:   使后面的内容脱离文档流
/* right   left   both*/
#foo{			
	clear:right;
			}
相对定位;  poisition : relative
		 没有脱离文档流 对兄弟元素没有影响 相对于元素原来的位置进行定位
/*块级元素 (block)   行级元素(内联元素   inline)*/
				/*static 正常文档流  (默认模式)   folat */
				/* relative 相对定位    相对原来的位置偏离位置   没有脱离文档流*/
abslout  绝对定位:相对于父元素来说设定位置  脱离的正常的文档流
		对兄弟元素有影响(相当于其不存在的情况下其兄弟元素进行补位了)
fixed  固定定位:相对于浏览器窗口来设定元素原来的位置  脱离了文档流
Z-index: num;  数字越大 最后出现 数字小的会被大的覆盖

处理塌陷问题   	
<div style="clear: both;"></div>  空白 div 用于方便计算 div高度
ovreflow: auto;    设置自动溢出文档
#foo {   /* 在设置最外层块时添加 overflow:auto; */
	overflow: auto; /* css hack 使外框显示出来*/
	}
```

