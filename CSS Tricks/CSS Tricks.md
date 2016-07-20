#背景与边框
##半透明边框
尝试使用HSLA设定_border_的颜色为半透明，结果发现会被背景色覆盖。这是由于CSS`background-clip`属性的默认值为_border-box_所导致的。
>**background-clip:**  用于规定背景图的绘制区域，默认值为*border-box*。即背景图会出现在padding和border这两层当中。
因此，为了能够达到边框半透明的效果我们需要重新设定`background-clip`的值：
```CSS
div.content{
  width: 200px;
  margin: 0 auto;
  background: #fff;
  padding: 10px;
  border: 10px solid hsla(0, 0%, 100%, 0.5);
  background-clip: padding-box;
}
```
[半透明边框示例地址](http://codepen.io/Narcissus_Liu/pen/zBRrRq)
[background-clip CSS Trick 相关介绍](https://css-tricks.com/almanac/properties/b/background-clip/)

##多重边框
**box-shadow:** 创建边框阴影。其优点在于可以支持逗号分隔语法，创建任意数量的投影。
`box-shadow: offset-x | offset-y | blur-radius | spread-radius  | color`;

> 使用`box-shadow`注意事项:
  1. `box-shadow`不同于`border`、`padding`和`margin`，它不会影响到布局，同时也不会被`box-sizing`所影响。
  2. `box-shadow`所产生的效果范围不会响应鼠标事件，即它向外所产生的效果范围都是虚的，不会被鼠标感知到。
  
```CSS
.more-border{
  width: 200px;
  margin: 0 auto;
  background: yellowgreen;
  padding: 10px;
  box-shadow: 0 0 10px #655, 
              0 0 15px deeppink,
              0 2px 5px 15px rgba(0 , 0, 0, 0.6);
  
}
```
[多边框示例](http://codepen.io/Narcissus_Liu/pen/kXZzrk)
[box-shadow MDN相关资料](https://developer.mozilla.org/zh-CN/docs/Web/CSS/box-shadow)

#视觉效果
##毛玻璃效果
**相关知识点**
`background-attachment: scroll(default) | fixed;` 设定背景图像是否固定或者随着页面其余部分滚动。

`overflow: visible(default) | hidden | scroll | auto `用于设定当内容溢出元素框时发生的动作。

`hsla() :`指色调(hue)、饱和度(saturation)、亮度(lightness)和透明度(alpha)。

`filter: `可以在元素呈现之前，为元素的渲染提供一些效果，如模糊、颜色转移之类的。滤镜常用于调整图像、背景、边框的渲染。此处渲染毛玻璃效果我们需要用到**`blur`**进行模糊处理。

首先，进行背景模糊处理我们通常会想到使用降低背景透明度的方式来进行处理。但是这么做的缺点在于同时会导致文本的可阅读性下降。所以选择借助`blur()`滤镜对元素进行模糊处理。

**`blur()`**进行模糊处理会从中心开始发散，在接近元素框边缘时逐渐消退。因此如果希望在元素框内尽量保持相同的模糊度可以尝试使用`filter:blur(20px)`的方式增加模糊处理的半径。然后使用`overflow:hidden`来剪切处理多出来的20px范围的模糊效果。

```CSS
body{
  min-height: 100vh;
	box-sizing: border-box;
	margin: 0;
	padding-top: calc(50vh - 6em);
	font: 150%/1.6 Baskerville, Palatino, serif;
  background: url(/img.png) 0 / cover fixed;//固定背景图，并选中cover的方式使图片充满背景。
}

.main::before{
  background: url(/img.png) 0 / cover fixed;//使用和背景图片完全相同图片，方便对文本后面的区域进行模糊处理。
}

main {
	position: relative;//为伪元素定位做准备。
	margin: 0 auto;//居中
	padding: 1em;
	max-width: 23em;
	background: hsla(0,0%,100%,.25) border-box;
	overflow: hidden;//切除溢出元素框的部分。
	border-radius: .3em;
	box-shadow: 0 0 0 1px hsla(0,0%,100%,.3) inset,
	            0 .5em 1em rgba(0, 0, 0, 0.6);
	text-shadow: 0 1px 1px hsla(0,0%,100%,.3);
}
//伪元素内容
main::before {
	content: '';
	position: absolute;//绝对定位
	top: 0; right: 0; bottom: 0; left: 0;//使伪元素完全覆盖在main元素上。
	margin: -30px;
	z-index: -1;//在main元素下层。
	-webkit-filter: blur(20px);//模糊处理，webkit用于在Chrome中生效。
	filter: blur(20px);
}
```



