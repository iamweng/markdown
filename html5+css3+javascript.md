x

# HTML5+CSS3+Javascript

# HTML5

## Semantics    // 语义化

### HTML标签

```html
// 定义注释
<!--...-->
// 定义文档类型
<!DOCTYPE>
// 定义超文本链接
<a>
// 定义缩写
<abbr>
// 定义文档作者或拥有者的联系信息
<address>
// HTML5中不赞成使用定义嵌入的 applet
<applet>
// 定义图像映射内部的区域
<area>
// 定义一个文章区域
<article>
// 定义页面的侧边栏内容
<aside>
// 定义音频内容
<audio>
// 定义文本粗体
<b>
// 定义页面中所有链接的默认地址或默认目标
<base>
// 允许您设置一段文本，使其脱离其父元素的文本方向设置
<bdi>
// 定义文字方向
<bdo>
// 定义长的引用
<blockquote>
// 定义文档的主体
<body>
// 定义换行
<br>
// 定义一个点击按钮
<button>
// 定义图形，比如图表和其他图像,标签只是图形容器，必须使用脚本来绘制图形
<canvas>
// 定义表格标题
<caption>
// 定义引用(citation)
<cite>
// 定义计算机代码文本
<code>
// 定义表格中一个或多个列的属性值
<col>
// 定义表格中供格式化的列组
<colgroup>
// 定义命令按钮，比如单选按钮、复选框或按钮
<command>
// 定义选项列表请与 input 元素配合使用该元素，来定义 input 可能的值
<datalist>
// 定义定义列表中项目的描述
<dd>
// 定义被删除文本
<del>
// 用于描述文档或文档某个部分的细节
<details>
// 定义定义项目
<dfn>
// 定义对话框，比如提示框
<dialog>
// 定义文档中的节
<div>
// 定义列表详情
<dl>
// 定义列表中的项目
 <dt>
// 定义强调文本
<em>
// 定义嵌入的内容，比如插件
<embed>
// 定义围绕表单中元素的边框
<fieldset>
// 定义<figure> 元素的标题
<figcaption>
// 规定独立的流内容（图像、图表、照片、代码等等）
<figure>
// 定义 section 或 document 的页脚
<footer>
// 定义HTML文档的表单
<form>
// 定义框架集的窗口或框架
<frame>
// 定义框架集
<frameset>
// 定义 HTML 标题
<h1> to <h6>
// 定义关于文档的信息
<head>
// 定义了文档的头部区域
<header>
// 定义水平线
<hr>
// 定义 HTML 文档
<html>
// 定义斜体字
<i>
// 定义内联框架
<iframe>
// 定义图像
<img>
// 定义输入控件
<input>
// 定义被插入文本
<ins>
// 定义键盘文本
<kbd>
// 规定用于表单的密钥对生成器字段
<keygen>
// 定义input元素的标注
<label>
// 定义 fieldset 元素的标题
<legend>
// 定义列表的项目
<li>
// 定义文档与外部资源的关系
<link>
// 定义文档的主体部分
<main>
// 定义图像映射
<map>
// 定义带有记号的文本请在需要突出显示文本时使用 <m> 标签
<mark>
// 不赞成使用定义菜单列表
<menu>
// 定义关于 HTML 文档的元信息
<meta>
// 定义度量衡仅用于已知最大和最小值的度量
<meter>
// 定义导航链接的部分
<nav>
// 定义针对不支持客户端脚本的用户的替代内容
<noscript>
// 定义内嵌对象
<object>
// 定义有序列表
<ol>
// 定义选择列表中相关选项的组合
<optgroup>
// 定义选择列表中的选项
<option>
// 定义不同类型的输出，比如脚本的输出
<output>
// 定义段落
<p>
// 定义对象的参数
<param>
// 定义预格式文本
<pre>
// 定义运行中的进度（进程）
<progress>
// 定义短的引用
<q>
// 在 ruby 注释中使用，以定义不支持 ruby 元素的浏览器所显示的内容
<rp>
// 定义字符（中文注音或字符）的解释或发音
<rt>
// 定义 ruby 注释（中文注音或字符）
<ruby>
// 不赞成使用定义加删除线的文本
<s>
// 定义计算机代码样本
<samp>
// 定义客户端脚本
\<script\>
// 定义文档中的节（section、区段）比如章节、页眉、页脚或文档中的其他部分
<section>
// 定义选择列表（下拉列表）
<select>
// 定义小号文本
<small>
// 媒介元素（比如 <video> 和 <audio>）定义媒介资源
<source>
// 定义文档中的节
<span>
// 定义强调文本
<strong>
// 定义文档的样式信息
<style>
// 定义下标文本
<sub>
// 包含 details 元素的标题，"details" 元素用于描述有关文档或文档片段的详细信息
<summary>
// 定义上标文本
<sup>
// 定义表格
<table>
// 定义表格中的主体内容
<tbody>
// 定义表格中的单元
<td>
// 定义多行的文本输入控件
<textarea>
// 定义表格中的表注内容（脚注）
<tfoot>
// 定义表格中的表头单元格
<th>
// 定义表格中的表头内容
<thead>
// 定义日期或时间，或者两者
<time>
// 定义文档的标题
<title>
// 定义表格中的行
<tr>
// 诸如 video 元素之类的媒介规定外部文本轨道
<track>
// 定义打字机文本
<tt>
// 不赞成使用定义下划线文本
<u>
// 定义无序列表
<ul>
// 定义文本的变量部分
<var>
// 定义视频，比如电影片段或其他视频流
<video>
// 规定在文本中的何处适合添加换行符
<wbr>
```

### 全局属性

```css
// 设置访问元素的键盘快捷键
accesskey
// 规定元素的类名（classname）
class
// 规定是否可编辑元素的内容
contenteditable
// 指定一个元素的上下文菜单。当用户右击该元素，出现上下文菜单
contextmenu
// 用于存储页面的自定义数据
data-*
// 设置元素中内容的文本方向
dir
// 指定某个元素是否可以拖动
draggable
// 指定是否将数据复制，移动，或链接，或删除
dropzone
// 属性规定对元素进行隐藏
hidden
// 规定元素的唯一 id
id
// 设置元素中内容的语言代码。
lang
// 检测元素是否拼写错误
spellcheck
// 规定元素的行内样式（inline style）
style
// 设置元素的 Tab 键控制次序。
tabindex
// 规定元素的额外信息（可在工具提示中显示）
title
// 指定是否一个元素的值在页面载入时是否需要翻译
translate
```



## Offline & Storage    // 离线 & 存储

#### Web Storage // 网页存储

##### window.localStorage(20M) // 本地存储

存储在硬盘中，永久性存储;

##### window.sessionStorge(5M) // 区域存储

存储在浏览器内存中，关闭浏览器后清除，只在当前浏览器窗口有效;

#### ApplicationCache //  应用缓存

##### manifest.appcache // 清单文件

```manifest
CACHE MANIFEST

// 列出的文件将在首次下载后进行缓存
CACHE:
// 列出的文件需要与服务器连接,且不会被缓存
NETWORK:
// 列出的文件规定当页面无法访问时的回退页面
FALLBACK:
```

```html
// 指定清单文件路径
<html manifest="/?.appcache"></html>
```

```
// 未缓存 0
appCache.UNCACHED
// 闲置 1
appCache.IDLE
// 检查中 2
appCache.CHECKING
// 下载中 3
appCache.DOWNLOADING
// 已更新 4
appCache.UPDATEREADY
// 失效 5
appCache.OBSOLETE
```

```javascript
// 发起应用程序缓存下载进程
update()
// 取消正在进行的缓存下载
abort()
// 切换成本地最近的缓存环境
swapcahce()
```

## Device Access    // 设备访问

## Connectivity // 连接性

## Multimedia   // 多媒体

## Graphics & Effects   // 图形 & 特效

## Performance & Integration    // 性能 & 整合

# CSS3

### 动画属性

```css
// 定义一个动画,@keyframes定义的动画名称用来被animation-name所使用
@keyframes animationname {keyframes-selector {css-styles;}}
// 复合属性 检索或设置对象所应用的动画特效
animation::name duration timing-function delay iteration-count direction fill-mode play-state
// 检索或设置对象所应用的动画名称 ,必须与规则@keyframes配合使用，因为动画名称由@keyframes定义
animation-name:keyframename|none
// 检索或设置对象动画的持续时间
animation-duration:time
// 检索或设置对象动画的过渡类型
animation-timing-function:linear|ease|ease-in|ease-out|ease-in-out|cubic-bezier(n,n,n,n)
// 检索或设置对象动画的延迟时间
animation-delay:time
// 检索或设置对象动画的循环次数
animation-iteration-count:n|infinite
// 检索或设置对象动画在循环中是否反向运动
animation-direction:normal|reverse|alternate|alternate-reverse|initial|inherit
// 检索或设置对象动画的状态
animation-play-state:paused|running
```

### 背景属性

```css
// 复合属性设置对象的背景特性
background:bg-color bg-image position/bg-size bg-repeat bg-origin bg-clip bg-attachment initial|inherit
// 设置或检索背景图像是随对象内容滚动还是固定的 必须先指定background-image属性
background-attachment:scroll|fixed|local|initial|inherit
// 设置或检索对象的背景颜色
background-color:color
// 设置或检索对象的背景图像
background-image:url()
// 设置或检索对象的背景图像位置。必须先指定background-image属性
background-position
// 设置或检索对象的背景图像如何铺排填充。必须先指定background-image属性
background-repeat:repeat|repeat-x|repeat-y|no-repeat|inherit
// 指定对象的背景图像向外裁剪的区域
background-clip:border-box|padding-box|content-box
// 设置或检索对象的背景图像计算background-position时的参考原点(位置)
background-origin:padding-box|border-box|content-box
// 检索或设置对象的背景图像的尺寸大小
background-size:length|percentage|cover|contain
```

### 边框和轮廓属性

```css
// 复合属性设置对象边框的特性
border：border-width border-style border-color|inherit
// 复合属性设置对象底部边框的特性
border-bottom:border-bottom-width border-bottom-style border-bottom-color|inherit
// 设置或检索对象的底部边框颜色
border-bottom-color:color|transparent|inherit
// 设置或检索对象的底部边框样式
border-bottom-style:none|hidden|dotted|dashed|soild|double|groove
// 设置或检索对象的底部边框宽度
border-bottom-width:thin|medium|thick|length|inherit
// 设置或检索对象的边框颜色
border-color:color|transparent|inherit
// 复合属性设置对象左边边框的特性
border-left:border-left-width border-left-style border-left-color|inherit
// 设置或检索对象的左边边框颜色
border-left-color:color|transparent|inherit
// 设置或检索对象的左边边框样式
border-left-style:none|hidden|dotted|dashed|soild|double|groove
// 设置或检索对象的左边边框宽度
border-left-width:thin|medium|thick|length|inherit
// 复合属性设置对象右边边框的特性
border-right:border-right-width border-right-style border-right-color|inherit
// 设置或检索对象的右边边框颜色
border-right-color:color|transparent|inherit
// 设置或检索对象的右边边框样式
border-right-style:none|hidden|dotted|dashed|soild|double|groove
// 设置或检索对象的右边边框宽度
border-right-width:thin|medium|thick|length|inherit
// 设置或检索对象的边框样式
border-style:none|hidden|dotted|dashed|soild|double|groove
// 复合属性设置对象顶部边框的特性
border-top:border-top-width border-top-style border-top-color|inherit
// 设置或检索对象的顶部边框颜
border-top-color:color|transparent|inherit
// 设置或检索对象的顶部边框样式
border-top-style:none|hidden|dotted|dashed|soild|double|groove
// 设置或检索对象的顶部边框宽度
border-top-width:thin|medium|thick|length|inherit
// 设置或检索对象的边框宽度
border-width:thin|medium|thick|length|inherit
// 复合属性设置或检索对象外的线条轮廓
outline:outline-color outline-style outline-width|inherit
// 设置或检索对象外的线条轮廓的颜色
outline-color:color|invert|inherit
// 设置或检索对象外的线条轮廓的样式
outline-style:none|hidden|dotted|dashed|soild|double|groove
// 设置或检索对象外的线条轮廓的宽度
outline-width:thin|medium|thick|length|inherit
// 设置或检索对象的左下角圆角边框
border-bottom-left-radius:length|%
// 设置或检索对象的右下角圆角边框
border-bottom-right-radius:length|%
// 设置或检索对象的边框样式使用图像来填充
border-image:source slice width outset repeat|initial|inherit
// 规定边框图像超过边框的量
border-image-outset:length|number
// 规定图像边框是否应该被重复（repeated）、拉伸（stretched）或铺满（rounded）
border-image-repeat:stretch|repeat|round|initial|inherit
// 规定图像边框的向内偏移
border-image-slice:number|%|fill
// 规定要使用的图像，代替 border-style 属性中设置的边框样式
border-image-source:none|image
// 规定图像边框的宽度
border-image-width:number|%|auto
// 设置或检索对象使用圆角边框
border-radius:length|%
// 定义左上角边框的形状
border-top-left-radius:length|%
// 定义右上角边框的形状
border-top-right-radius:length|%
// 规定行内元素被折行
box-decoration-break
// 向方框添加一个或多个阴影
box-shadow:h-shadow v-shadow blur spread color inset
```

### 盒子属性

```css
// 如果内容溢出了元素内容区域，是否对内容的左/右边缘进行裁剪
overflow-x:visible|hidden|scroll|auto|no-display|no-content
// 如果内容溢出了元素内容区域，是否对内容的上/下边缘进行裁剪
overflow-y:visible|hidden|scroll|auto|no-display|no-content
// 规定溢出元素的首选滚动方法
overflow-style:auto|scrollbar|panner|move|marquee
// 围绕由 rotation-point 属性定义的点对元素进行旋转
rotation:angle
// 定义距离上左边框边缘的偏移点
rotation-point:value
```

### 颜色属性

```css
// 允许使用源的颜色配置文件的默认以外的规范
color-profile
// 设置一个元素的透明度级别
opacity:value|inherit
// 允许超过默认颜色配置文件渲染意向的其他规范
rendering-intent
```

### 内边距属性

```css
// 在一个声明中设置所有填充属性
padding:length|%|inherit
// 设置元素的底填充
padding-bottom:length|%|inherit
// 设置元素的左填充
padding-left:length|%|inherit
// 设置元素的右填充
padding-right:length|%|inherit
// 设置元素的顶部填充
padding-top:length|%|inherit
```

### 媒体页面内容属性

```css
// 指定书签的标签
bookmark-label
// 指定了书签级别
bookmark-level
// 指定了书签链接的目标
bookmark-target
// 在相反的方向推动浮动元素，他们一直具有浮动
float-offset
// 指定一个断字的单词断字字符后的最少字符数
hyphenate-after
// 指定一个断字的单词断字字符前的最少字符数
hyphenate-before
// 指定了当一个断字发生时，要显示的字符串
hyphenate-character
// 表示连续断字的行在元素的最大数目
hyphenate-lines
// 外部资源指定一个逗号分隔的列表，可以帮助确定浏览器的断字点
hyphenate-resource
// 设置如何分割单词以改善该段的布局
hyphens
// 指定了正确的图像分辨率
image-resolution
// 将crop and/or cross标志添加到文档
marks
string-set
```

### 尺寸属性

```css
// 设置元素的高度
height:auto|length|%|inherit
// 设置元素的最大高度
max-height:auto|length|%|inherit
// 设置元素的最大宽度
max-width:auto|length|%|inherit
// 设置元素的最小高度
min-height:auto|length|%|inherit
// 设置元素的最小宽度
min-width:auto|length|%|inherit
// 设置元素的宽度
width:auto|length|%|inherit
```

### 弹性盒子模型属性

```css
// 复合属性 设置或检索弹性盒模型对象的子元素如何分配空间
flex:flex-grow flex-shrink flex-basis|auto|initial|inherit
// 设置或检索弹性盒的扩展比率
flex-grow:number|initial|inherit
// 设置或检索弹性盒的收缩比率
flex-shrink:number|initial|inherit
// 设置或检索弹性盒伸缩基准值
flex-basis:number|auto|initial|inherit
// 复合属性 设置或检索弹性盒模型对象的子元素排列方式
flex-flow:flex-direction flex-wrap|initial|inherit
// 该属性通过定义flex容器的主轴方向来决定felx子项在flex容器中的位置
flex-direction:row|row-reverse|column|column-reverse|initial|inherit
// 该属性控制flex容器是单行或者多行，同时横轴的方向决定了新行堆叠的方向
flex-wrapL:nowrap|wrap|wrap-reverse|initial|inherit
// 在弹性容器内的各项没有占用交叉轴上所有可用的空间时对齐容器内的各项（垂直）
align-content:stretch|center|flex-start|flex-end|space-between|space-around|initial|inherit
// 定义flex子项在flex容器的当前行的侧轴（纵轴）方向上的对齐方式
align-items:stretch|center|flex-start|flex-end|baseline|initial|inherit
// 定义flex子项单独在侧轴（纵轴）方向上的对齐方式
align-self:auto|stretch|center|flex-start|flex-end|baseline|initial|inherit
// 设置或检索弹性盒子元素在主轴（横轴）方向上的对齐方式
justify-content:flex-start|flex-end|center|space-between|space-around|initial|inherit
// 设置或检索弹性盒模型对象的子元素出现的順序
order:number|initial|inherit
```

### 字体属性

```css
// 在一个声明中设置所有字体属性
font:font-style font-variant font-weight font-size font-family caption
// 规定文本的字体系列
font-family:family-name|gengric-family
// 规定文本的字体尺寸
font-size:mall|medium|lagre|length|%|inherit
// 规定文本的字体样式
font-style:normal|italic|oblique|inherit
// 规定文本的字体样式
font-variant:normal|small-caps|inherit
// 规定字体的粗细
font-weight:normal|bold|bolder|lighter|number
// 一个规则，允许网站下载并使用其他超过"Web- safe"字体的字体
@font-face
// 为元素规定 aspect 值
font-size-adjust:number|none|inherit
// 收缩或拉伸当前的字体系列
font-stretch:wider|narrower|ultra-condensed|extra-condensed|condensed|semi-condensed|normal|semi-expanded|expanded|extra-expanded|ultra-expanded|inherit
```

### 内容生成属性

```css
// 与:before以及:after 伪元素配合使用，来插入生成内容
content:none|normal|counter|attr|string
// 递增或递减一个或多个计数器
counter-increment:none|id number|inherit
// 创建或重置一个或多个计数器
counter-reset:none|id number|inherit
// 设置嵌套引用的引号类型
quotes:none|string string string string|inhert
// 允许replaced元素只是作为一个对象代替整个对象的矩形区域
crop
// 从流中删除元素，然后在文档中后面的点上重新插入
move-to
// 判定基于页面的给定元素的适用于计数器的字符串值
page-policy
```

### 网格属性

```css
// 指定在网格中每列的宽度
grid-columns:length|%|none|inherit
// 指定在网格中每列的高度
grid-rows:length|%|none|inherit
```

### 超链接属性

```css
// 简写属性设置target-name, target-new,和target-position属性
target:target-name target-new target-position
// 指定在何处打开链接（目标位置）
target-name:current|root|parent|new|modal|name
// 指定是否有新的目标链接打开一个新窗口或在现有窗口打开新标签
target-new:window|tab|none
// 指定应该放置新的目标链接的位置
target-position:above|behind|front|back
```

### 线框属性

```css
// 允许更精确的元素的对齐方式
alignment-adjust
// 其父级指定的内联级别的元素如何对齐
alignment-baseline
// 允许重新定位相对于dominant-baseline的dominant-baseline
baseline-shift
// 指定scaled-baseline-table
dominant-baseline
// 设置下拉的主要连接点的初始对齐点
drop-initial-after-adjust
// 校准行内的初始行的设置就是具有首字母的框使用初级连接点
drop-initial-after-align
// 设置下拉的辅助连接点的初始对齐点
drop-initial-before-adjust
// 校准行内的初始行的设置就是具有首字母的框使用辅助连接点
drop-initial-before-align
// 控制局部的首字母下沉
drop-initial-size
// 激活一个下拉式的初步效果
drop-initial-value
// 设置一个多行的内联块内的行具有前一个和后一个内联元素的对齐
inline-box-align
// 一个速记属性设置line-stacking-strategy, line-stacking-ruby,和line-stacking-shift属性
line-stacking
// 设置包含Ruby注释元素的行对于块元素的堆叠方法
line-stacking-ruby
// 设置base-shift行中块元素包含元素的堆叠方法
line-stacking-shift
// 设置内部包含块元素的堆叠线框的堆叠方法
line-stacking-strategy
// 行内框的文本内容区域设置block-progression维数
text-height
```

### 列表属性

```css
// 在一个声明中设置所有的列表属性
list-style:list-style-type|list-style-position|list-style-image|initial|inherit
// 将图象设置为列表项标记
list-style-image:URL|none|inherit
// 设置列表项标记的放置位置
list-style-position:inside|outside|inherit
// 设置列表项标记的类型
list-style-type:none|dis|circle|square|decimal
```

### 外边距属性

```css
// 在一个声明中设置所有外边距属性
margin:auto|length|%|inherit
// 设置元素的下外边距
margin-bottom:auto|length|%|inherit
// 设置元素的左外边距
margin-left:auto|length|%|inherit
// 设置元素的右外边距
margin-right:auto|length|%|inherit
// 设置元素的上外边距
margin-top:auto|length|%|inherit
```

### 字幕属性

```css
// 设置内容移动的方向
marquee-direction
// 设置内容移动多少次
marquee-play-count
// 设置内容滚动的速度有多快
marquee-speed
// 设置内容移动的样式
marquee-style
```

### 多列属性

```css
// 指定元素应该分为的列数
column-count:number|auto
// 指定如何填充列
column-fill:number|auto
// 指定列之间的差距
column-gap:length|normal
// 对于设置所有column-rule-*属性的简写属性
column-rule:column-rule-width column-rule-style column-rule-color
// 指定列之间的颜色规则
column-rule-color:color
// 指定列之间的样式规则
column-rule-style:none|hidden|dotted|dashed|solid|double|groove|ridge|inset|outset
// 指定列之间的宽度规则
column-rule-width:thin|medium|thick|length
// 指定元素应该跨越多少列
column-span:number|all
// 指定列的宽度
column-width:auto|length
// 缩写属性设置列宽和列数
columns:column-width column-count
```

### 页面媒体属性

```css
// 如果其宽度和高度属性都不是auto给出一个提示，如何大规模替换元素
fit
// 判定方框内对象的对齐方式
fit-position
// 指定用户代理适用于图像中的向右或顺时针方向的旋转
image-orientation
// 指定一个元素应显示的页面的特定类型
page
// 指定含有BOX的页面内容的大小和方位
size
```

### 定位属性

```css
// 设置定位元素下外边距边界与其包含块下边界之间的偏移
bottom:auto|%|length|inherit
// 规定元素的哪一侧不允许其他浮动元素
clear:left|right|both|none|inherit
// 剪裁绝对定位元素
clip:rect(top,right,bottom,left)|auto|inherit
// 规定要显示的光标的类型（形状）
cursor
// 规定元素应该生成的框的类型
display:none|block|inline|inline-block|inherit
// 规定框是否应该浮动
float:left|right|none|inherit
// 设置定位元素左外边距边界与其包含块左边界之间的偏移
left:auto|%|length|inherit
// 规定当内容溢出元素框时发生的事情
overflow:visible|hidden|scroll|auto|inherit
// 规定元素的定位类型
position:absolute|fixed|reltive|static|sticky|inherit|initial
// 设置定位元素右外边距边界与其包含块右边界之间的偏移
right:auto|%|length|inherit
// 设置定位元素的上外边距边界与其包含块上边界之间的偏移
top:auto|%|length|inherit
// 规定元素是否可见
visibility:visible|hidden|collapse|inherit
// 设置元素的堆叠顺序
z-index:auto|number|inherit
```

### 分页属性

```css
orphans // 设置当元素内部发生分页时必须在页面底部保留的最少行数
page-break-after:auto|always|avoid|left|right|inherit   // 设置元素后的分页行为
page-break-before:auto|always|avoid|left|right|inherit  // 设置元素前的分页行为
page-break-inside:auto|avoid|inherit    // 设置元素内部的分页行为
widows  // 设置当元素内部发生分页时必须在页面顶部保留的最少行数
```

### Ruby属性

```css
// 控制Ruby文本和Ruby基础内容相对彼此的文本对齐方式
ruby-align
// 当Ruby文本超过Ruby的基础宽，确定ruby文本是否允许局部悬置任意相邻的文本，除了自己的基础
ruby-overhang
// 它的base控制Ruby文本的位置
ruby-position
// 控制annotation 元素的跨越行为
ruby-span
```

### 语音属性

```css
// 缩写属性设置mark-before和mark-after属性
mark
// 允许命名的标记连接到音频流
mark-after
// 允许命名的标记连接到音频流
mark-before
// 指定包含文本的相应元素中的一个音标发音
phonemes
// 一个缩写属性设置rest-before和rest-after属性
rest
// 一个元素的内容讲完之后，指定要休息一下或遵守韵律边界
rest-after
// 一个元素的内容讲完之前，指定要休息一下或遵守韵律边界
rest-before
// 指定了左，右声道之间的平衡
voice-balance
// 指定应采取呈现所选元素的内容的长度
voice-duration
// 指定平均说话的声音的音调（频率）
voice-pitch
// 指定平均间距的变化
voice-pitch-range
// 控制语速
voice-rate
// 指示着重力度
voice-stress
// 语音合成是指波形输出幅度
voice-volume
```

### 表格属性

```css
// 规定是否合并表格边框
border-collapse:collapse|separate|inherit
// 规定相邻单元格边框之间的距离
border-spacing:length length|inherit
// 规定表格标题的位置
caption-side:top|bottom|inherit
// 规定是否显示表格中的空单元格上的边框和背景
empty-cells:hide|show|inherit
// 设置用于表格的布局算法
table-layout:automatic|fixed|inherit
```

### 文本属性

```css
// 设置文本的颜色
color:color_name|hex_number|rgb_number|inherit
// 规定文本的方向 书写方向
direction:ltr|rtl|inherit
// 设置字符间距
letter-spacing:normal|length|inherit
// 设置行高
line-height:normal|number|length|%|inherit
// 规定文本的水平对齐方式
text-align:left|right|center|justify|inherit
// 规定添加到文本的装饰效果
text-decoration:none|underline|overline|line-through|blink|inherit
// 规定文本块首行的缩进
text-indent:length|%|inherit
// 控制文本的大小写
text-transform|none|captalize|uppercase|lowerase|inherit
unicode-bidi
// 设置元素的垂直对齐方式
vertical-align:baseline|sub|super|top|text-top|middle|bottom|text-bottom|length|%|inherit
// 设置怎样给一元素控件留白
white-space:normal|ore|nowrap|pre-wrap|pre-line|inherit
// 设置单词间距
word-spacing:normal|length|inherit
// 向元素的文本应用重点标记以及重点标记的前景色
text-emphasis:text-emphasis-style text-emphasis-color
// 指定一个标点符号是否可能超出行框
hanging-punctuation:none|first|last|allow-end|force-end
// 指定一个标点符号是否要去掉
punctuation-trim:none|start|end|allow-end|adjacent
// 当 text-align 设置为 justify 时，最后一行的对齐方式
text-align-last:auto|left|right|center|justify|start|end|initial|inherit
/当 text-align 设置为 justify 时指定分散对齐的方式
text-justify:auto|inter-word|inter-ideograph|inter-cluster|distribute|kashida|trim
// 设置文字的轮廓
text-outline:thickness blur color
// 指定当文本溢出包含的元素，应该发生什么
text-overflow:clip|ellipsis|string
// 为文本添加阴影
text-shadow:h-shadow v-shadow blur color
// 指定文本换行规则
text-wrap:normal|none|unrestricted|suppres
// 指定非CJK文字的断行规则
word-break:normal|break-all|keep-all
// 设置浏览器是否对过长的单词进行换行
word-wrap:normal|break-word
```

### 2D|3D转换属性

```css
// 适用于2D或3D转换的元素
transform:none|transform-functions
// 允许您更改转化元素位置
transform-origin:x-axis y-axis z-axis
// 3D空间中的指定如何嵌套元素
transform-style:flat|preserve-3d
// 指定3D元素是如何查看透视图
perspective:number|none
// 指定3D元素底部位置
perspective-origin:x-axis y-axis
// 定义一个元素是否应该是可见的，不对着屏幕时
backface-visibility:visible|hidden
```

### 过渡属性

```css
// 此属性是 transition-property、transition-duration、transition-timing-function、transition-delay的简写形式
transition:property duration timing-function delay
// 设置用来进行过渡的 CSS 属性
transition-property:none|all| property
// 设置过渡进行的时间长度
transition-duration:time
// 设置过渡进行的时序函数
transition-timing-function:linear|ease|ease-in|ease-out|ease-in-out|cubic-bezier(n,n,n,n)
// 指定过渡开始的时间
transition-delay:time
```

### 用户外观属性

```css
appearance:normal|icon|window|button|menu|field // 定义元素的外观样式
// 允许您为了适应区域以某种方式定义某些元素
box-sizing:content-box|border-box|inherit
// 为元素指定图标
icon:auto|URL|inherit
// 指定用户按向下键时向下导航的位置
nav-down:auto|id|target-name|inherit
// 指定导航（tab）顺序
nav-index:auto|number|inherit
// 指定用户按向左键时向左导航的位置
nav-left:auto|id|target-name|inherit
// 指定用户按向右键时向左导航的位置
nav-right:auto|id|target-name|inherit
// 指定用户按向上键时向上导航的位置
nav-up:auto|id|target-name|inherit
// 设置轮廓框架在 border 边缘外的偏移
outline-offset:length|inherit
// 定义元素是否可以改变大小
resize:none|both|horizontal|vertical
```

### 媒体查询属性

```css
// 用于所有设备
all
// 用于打印机和打印预览
print
// 用于电脑屏幕，平板电脑，智能手机等。
screen
// 应用于屏幕阅读器等发声设备
speech
```

```css
// 定义输出设备中的页面可见区域宽度与高度的比率
aspect-ratio
// 定义输出设备每一组彩色原件的个数。如果不是彩色设备，则值等于0
color
// 定义在输出设备的彩色查询表中的条目数。如果没有使用彩色查询表，则值等于0
color-index
// 定义输出设备的屏幕可见宽度与高度的比率
device-aspect-ratio
// 定义输出设备的屏幕可见高度
device-height
// 定义输出设备的屏幕可见宽度
device-width
// 用来查询输出设备是否使用栅格或点阵
grid
// 定义输出设备中的页面可见区域高度
height
// 定义输出设备的屏幕可见宽度与高度的最大比率
max-aspect-ratio
// 定义输出设备每一组彩色原件的最大个数
max-color
// 定义在输出设备的彩色查询表中的最大条目数
max-color-index
// 定义输出设备的屏幕可见宽度与高度的最大比率
max-device-aspect-ratio
// 定义输出设备的屏幕可见的最大高度
max-device-height
// 定义输出设备的屏幕最大可见宽度
max-device-width
// 定义输出设备中的页面最大可见区域高度
max-height
// 定义在一个单色框架缓冲区中每像素包含的最大单色原件个数
max-monochrome
// 定义设备的最大分辨率
max-resolution
// 定义输出设备中的页面最大可见区域宽度
max-width
// 定义输出设备中的页面可见区域宽度与高度的最小比率
min-aspect-ratio
// 定义输出设备每一组彩色原件的最小个数
min-color
// 定义在输出设备的彩色查询表中的最小条目数
min-color-index
// 定义输出设备的屏幕可见宽度与高度的最小比率
min-device-aspect-ratio
// 定义输出设备的屏幕最小可见宽度
min-device-width
// 定义输出设备的屏幕的最小可见高度
min-device-height
// 定义输出设备中的页面最小可见区域高度
min-height
// 定义在一个单色框架缓冲区中每像素包含的最小单色原件个数
min-monochrome
// 定义设备的最小分辨率
min-resolution
// 定义输出设备中的页面最小可见区域宽度
min-width
// 定义在一个单色框架缓冲区中每像素包含的单色原件个数。如果不是单色设备，则值等于0
monochrome
// 定义输出设备中的页面可见区域高度是否大于或等于宽度
orientation
// 定义设备的分辨率。如：96dpi, 300dpi, 118dpcm
resolution
// 定义电视类设备的扫描工序
scan
// 定义输出设备中的页面可见区域宽度
width
```



# Javascript

## 触发事件

### 窗口触发事件

```javascript
// 在打印文档之后运行脚本
onafterprint
// 在文档打印之前运行脚本
onbeforeprint
// 在文档加载之前运行脚本
onbeforeonload
// 当窗口失去焦点时运行脚本
onblur
// 当错误发生时运行脚本
onerror
// 当窗口获得焦点时运行脚本
onfocus
// 当文档改变时运行脚本
onhaschange
// 当文档加载时运行脚本
onload
// 当触发消息时运行脚本
onmessage
// 当文档离线时运行脚本
onoffline
// 当文档上线时运行脚本
ononline
// 当窗口隐藏时运行脚本
onpagehide
// 当窗口可见时运行脚本
onpageshow
// 当窗口历史记录改变时运行脚本
onpopstate
// 当文档执行再执行操作（redo）时运行脚本
onredo
// 当调整窗口大小时运行脚本
onresize
// 当 Web Storage 区域更新时（存储空间中的数据发生变化时）运行脚本
onstorage
// 当文档执行撤销时运行脚本
onundo
// 当用户离开文档时运行脚本
onunload
```

### 表单触发事件

```javascript
// 当元素失去焦点时运行脚本
onblur
// 当元素改变时运行脚本
onchange
// 当触发上下文菜单时运行脚本
oncontextmenu
// 当元素获得焦点时运行脚本
onfocus
// 当表单改变时运行脚本
onformchange
// 当表单获得用户输入时运行脚本
onforminput
// 当元素获得用户输入时运行脚本
oninput
// 当元素无效时运行脚本
oninvalid
// 当选取元素时运行脚本
onselect
// 当提交表单时运行脚本
onsubmit
```

### 键盘触发事件

```javascript
// 当按下按键时运行脚本
onkeydown
// 当按下并松开按键时运行脚本
onkeypress
// 当松开按键时运行脚本
onkeyup
```

### 鼠标触发事件

```javascript
// 当单击鼠标时运行脚本
onclick
// 当双击鼠标时运行脚本
ondblclick
// 当拖动元素时运行脚本
ondrag
// 当拖动操作结束时运行脚本
ondragend
// 当元素被拖动至有效的拖放目标时运行脚本
ondragenter
// 当元素离开有效拖放目标时运行脚本
ondragleave
// 当元素被拖动至有效拖放目标上方时运行脚本
ondragover
// 当拖动操作开始时运行脚本
ondragstart
// 当被拖动元素正在被拖放时运行脚本
ondrop
// 当按下鼠标按钮时运行脚本
onmousedown
// 当鼠标指针移动时运行脚本
onmousemove
// 当鼠标指针移出元素时运行脚本
onmouseout
// 当鼠标指针移至元素之上时运行脚本
onmouseover
// 当松开鼠标按钮时运行脚本
onmouseup
// 当转动鼠标滚轮时运行脚本
onmousewheel
// 当滚动元素的滚动条时运行脚本
onscroll
```

### 多媒体触发事件

```javascript
// 当发生中止事件时运行脚本
onabort
// 当媒介能够开始播放但可能因缓冲而需要停止时运行脚本
oncanplay
// 当媒介能够无需因缓冲而停止即可播放至结尾时运行脚本
oncanplaythrough
// 当媒介长度改变时运行脚本
ondurationchange
// 当媒介资源元素突然为空时（网络错误、加载错误等）运行脚本
onemptied
// 当媒介已抵达结尾时运行脚本
onended
// 当在元素加载期间发生错误时运行脚本
onerror
// 当加载媒介数据时运行脚本
onloadeddata
// 当媒介元素的持续时间以及其他媒介数据已加载时运行脚本
onloadedmetadata
// 当浏览器开始加载媒介数据时运行脚本
onloadstart
// 当媒介数据暂停时运行脚本
onpause
// 当媒介数据将要开始播放时运行脚本
onplay
// 当媒介数据已开始播放时运行脚本
onplaying
// 当浏览器正在取媒介数据时运行脚本
onprogress
// 当媒介数据的播放速率改变时运行脚本
onratechange
// 当就绪状态（ready-state）改变时运行脚本
onreadystatechange
// 当媒介元素的定位属性 [1] 不再为真且定位已结束时运行脚本
onseeked
// 当媒介元素的定位属性为真且定位已开始时运行脚本
onseeking
// 当取回媒介数据过程中（延迟）存在错误时运行脚本
onstalled
// 当浏览器已在取媒介数据但在取回整个媒介文件之前停止时运行脚本
onsuspend
// 当媒介改变其播放位置时运行脚本
ontimeupdate
// 当媒介改变音量亦或当音量被设置为静音时运行脚本
onvolumechange
// 当媒介已停止播放但打算继续播放时运行脚本
onwaiting
```

### 其他事件

```javascript
// 当 <menu> 元素在上下文显示时触发
onshow
// 当用户打开或关闭 <details> 元素时触发
ontoggle
```

## Canvas

```javascript
var canvas=document.getElementById("canvas");
// 获取上下文对象
var context = canvas.getContext("2d");
```

### 颜色 & 样式 & 阴影

```javascript
// 设置或返回用于填充绘画的颜色、渐变或模式
fillStyle=color|gradient对象|pattern对象
// 设置或返回用于笔触的颜色、渐变或模式
strokeStyle=color|gradient对象|pattern对象
// 设置或返回用于阴影的颜色
shadowColor=color
// 设置或返回用于阴影的模糊级别
shadowBlurr=number
// 设置或返回阴影与形状的水平距离
shadowOffsetX=number
// 设置或返回阴影与形状的垂直距离
shadowOffsetY=number

// 创建线性渐变（用在画布内容上）
createLinearGradient(x0,y0,x1,y1)
// 在指定的方向上重复指定的元素
createPattern(image|repeat|repeat-x|repeat-y|no-repeat)
// 创建放射状/环形的渐变（用在画布内容上）
createRadialGradient(x0,y0,r0,x1,y1,r1)
// 规定渐变对象中的颜色和停止位置
addColorStop(0.0-1.0,color)
```

### 线条样式

```javascript
// 设置或返回线条的结束端点样式
lineCap
// 设置或返回两条线相交时，所创建的拐角类型
lineJoin
// 设置或返回当前的线条宽度
lineWidth
// 设置或返回最大斜接长度
miterLimit
```

### 矩形

```javascript
// 创建矩形
rect(x,y,width,height)
// 绘制"被填充"的矩形
fillRect(x,y,width,height)
// 绘制矩形（无填充）
strokeRect(x,y,width,height)
// 在给定的矩形内清除指定的像素
clearRect(x,y,width,height)
```

### 路径

```javascript
// 填充当前绘图（路径）
fill()
// 绘制已定义的路径
stroke()
// 起始一条路径，或重置当前路径
beginPath()
// 把路径移动到画布中的指定点，不创建线条
moveTo()
// 创建从当前点回到起始点的路径。
closePath()
// 添加一个新点，然后在画布中创建从该点到最后指定点的线条
lineTo(x,y)
// 从原始画布剪切任意形状和尺寸的区域
clip()
// 创建二次贝塞尔曲线
quadraticCurveTo(cpx,cpy,x,y)
// 创建三次贝塞尔曲线。
bezierCurveTo(cp1x,cp1y,cp2x,cp2y,x,y)
// 创建弧/曲线（用于创建圆形或部分圆）
arc(x,y,r,sAngle,eAngle,counterclockwise)
// 创建两切线之间的弧/曲线
arcTo(x1,y1,x2,y2,r)
// 如果指定的点位于当前路径中，则返回 true，否则返回 false
isPointInPath(x,y)
```

### 转换

```javascript
// 缩放当前绘图至更大或更小
scale(scalewidth,scaleheight)
// 旋转当前绘图
rotate(angle)
// 重新映射画布上的 (0,0) 位置
translate(x,y)
// 替换绘图的当前转换矩阵
transform(a,b,c,d,e,f)
// 将当前转换重置为单位矩阵。然后运行 transform()
setTransform(a,b,c,d,e,f)
```

### 文本

```javascript
// 设置或返回文本内容的当前字体属性
font
// 设置或返回文本内容的当前对齐方式
textAlign="center|end|left|right|start"
// 设置或返回在绘制文本时使用的当前文本基线
textBaseline="alphabetic|top|hanging|middle|ideographic|bottom";

// 在画布上绘制"被填充的"文本。
fillText(text,x,y,maxWidth)
// 在画布上绘制文本（无填充）。
strokeText(text,x,y,maxWidth)
// 返回包含指定文本宽度的对象。
measureText(text).width
```

### 图像绘制

```javascript
// 向画布上绘制图像、画布或视频
drawImage(img,x,y)
// 向画布上绘制图像、画布或视频，并规定图像的宽度和高度
drawImage(img,x,y,width,height)
// 向画布上绘制图像、画布或视频，剪切图像，并在画布上定位被剪切的部分
drawImage(img,sx,sy,swidth,sheight,x,y,width,height)
```

### 像素操作

```javascript
// 返回 ImageData 对象的宽度
width
// 返回 ImageData 对象的高度
height
// 返回一个对象，其包含指定的 ImageData 对象的图像数据
data
```

### 合成

```javascript
// 设置或返回绘图的当前 alpha 或透明值
globalAlpha=number

// 设置或返回新图像如何绘制到已有的图像上
globalCompositeOperation=
// 默认 在目标图像上显示源图像
source-over
// 在目标图像顶部显示源图像
source-atop
// 在目标图像之内显示源图像
source-in
// 在目标图像之外显示源图像
source-out
// 在源图像上显示目标图像
destination-over
// 在源图像顶部显示目标图像
destination-atop
// 在源图像之内显示目标图像
destination-in
// 在源图像之外显示目标图像
destination-out
// 显示源图像 + 目标图像
lighter
// 显示源图像。忽略目标图像
copy
// 使用异或操作对源图像与目标图像进行组合
xor
```

### 其他

```javascript
// 保存当前环境的状态
save()
// 返回之前保存过的路径状态和属性
restore()
createEvent()
getContext()
toDataURL()
```

## Multimedia   // 多媒体

### HTML视频 & 音频

```javascript
// 向音频/视频添加新的文本轨道
addTextTrack(kind,label,language)
// 检测浏览器是否能播放指定的音频/视频类型
canPlayType(type)
// 重新加载音频/视频元素
load()
// 开始播放音频/视频
play()
// 暂停当前播放的音频/视频
pause()

// 返回表示可用音频轨道的 AudioTrackList 对象
audioTracks
// 设置或返回是否在加载完成后随即播放音频/视频
autoplay=true|false
// 返回表示音频/视频已缓冲部分的 TimeRanges 对象
buffered
// 返回表示音频/视频当前媒体控制器的 MediaController 对象
controller
// 设置或返回音频/视频是否显示控件（比如播放/暂停等）
controls=true|false
// 设置或返回音频/视频的 CORS 设置
crossOrigin
// 返回当前音频/视频的 URL
currentSrc
// 设置或返回音频/视频中的当前播放位置（以秒计）
currentTime="seconds"
// 设置或返回音频/视频默认是否静音
defaultMuted=true|false
// 设置或返回音频/视频的默认播放速度
defaultPlaybackRate=playbackspeed
// 返回当前音频/视频的长度（以秒计）
duration
// 返回音频/视频的播放是否已结束
ended
// 返回表示音频/视频错误状态的 MediaError 对象
error
// 设置或返回音频/视频是否应在结束时重新播放
loop=true|false
// 设置或返回音频/视频所属的组合（用于连接多个音频/视频元素）
mediaGroup="group"
// 设置或返回音频/视频是否静音
muted=true|false
// 返回音频/视频的当前网络状态
networkState
// 设置或返回音频/视频是否暂停
paused=true|false
// 设置或返回音频/视频播放的速度
playbackRate=playbackspeed
// 返回表示音频/视频已播放部分的 TimeRanges 对象
played
// 设置或返回音频|视频是否应该在页面加载后进行加载
preload="auto|metadata|none"
// 返回音频/视频当前的就绪状态
readyState
// 返回表示音频/视频可寻址部分的 TimeRanges 对象
seekable
// 返回用户是否正在音频/视频中进行查找
seeking
// 设置或返回音频/视频元素的当前来源
src=URL
// 返回表示当前时间偏移的 Date 对象
startDate
// 返回表示可用文本轨道的 TextTrackList 对象
textTracks
// 返回表示可用视频轨道的 VideoTrackList 对象
videoTracks
// 设置或返回音频/视频的音量
volume=volumevalue
```

## Geolocation  // 地理定位

```javascript
// 返回是否支持定位功能
navigator.geolocation
// 返回当前位置经纬度
navigator.geolocation.getCurrentPosition()
// 获得当前位置经纬度
navigator.geolocation。showPosition()
```

## Web Storage // 网页存储

```javascript
// 将JSON对象序列化
JSON.stringify(Object)
// 设置一个key
window.localStorage/(sessionStorge).serItem(key,value)
// 获取一个key
window.localStorage(sessionStorge).getItem(key)
// 删除一个key
window.localStorage(sessionStorge).removeItem(key)
// 访问Storage对象中item的对象数量
window.localStorage(sessionStorge).length
// 访问第n个key的名称
window.localStorage(sessionStorge).key(n)
// 清除当前域下的所有localStorage内容
window.localStorage(sessionStorge).clear()
```

```javascript
// 设置事件监听器
// 同源的网页才会触发此函数，导致数据变化的当前页面不会触发;
window.addEventListener("storage",function onStorageChange(event){
    window.alert(event.key);
});

// storage中被修改的键值
event.key
// storage中被修改前的值
event.oldValue
// storage中被修改后的值
event.newValue
// storage中值的页面的URL
event.url
// 变动的storage对象
event.storageArea
```

## Web SQL

```javascript
// 打开已存在的数据库，如果数据库不存在，则会创建一个新的数据库
openDatabase('DatabaseName','version','info','size',callback);
// 执行操作
database.transaction()
```

## Web Workers

```javascript
// 如果Web Worker可用就创建一个新的web worker对象
if(typeof(Worker)=="undefined")
{
    Worker=new Worker("workers.js");
}
// 终止web worker
Worker.terminate()
```

## Web Socket

```javascript
// 创建一个Web Socket对象
new WebSocket(url, [protocol] )
// 使用连接发送数据
Socket.send()
// 关闭连接
Socket.close()
```

### 触发事件

```javascript
// 连接建立时触发
Socket.onopen
// 客户端接收服务端数据时触发
Socket.onmessage
// 通信发生错误时触发
Socket.onerror
// 连接关闭时触发
ocket.onclose
```

