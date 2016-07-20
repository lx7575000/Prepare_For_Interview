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
###使用`outline`制作双层边框：
**outline: ** 用于设定四个边框的样式。
`outline: width style color;` ==> `outline: 5px solid red;`
```CSS
  .outline {
    width:200px;
    height: 150px;
    margin: 0 auto;
    background: yellowgreen;
    border: 10px solid #655;
    outline: 5px solid deeppink;
  }
```

缺点： 
1. outline只能用于制作双层边框。
2. outline产生的边框不一定会贴合border-radius产生的圆角

[多边框示例](http://codepen.io/Narcissus_Liu/pen/kXZzrk)
[box-shadow MDN相关资料](https://developer.mozilla.org/zh-CN/docs/Web/CSS/box-shadow)



