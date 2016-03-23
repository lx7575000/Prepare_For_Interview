#1. 单行文本水平垂直居中

```html
<div class='wrap'>
  水平居中水平居中水平居中
</div>
```

```css
html, body{
  margin: 0;
}
.wrap{
  line-height: 400px;
  text-align: center;
  
  height: 400px;
  font-size: 36px;
  background-color: #ccc;
}
```

#2. 利用盒模型的水平垂直居中

盒模型说白了就是，padding、border(可忽略)和margin。

具体的说法是，块级元素是由这些部分组成content-box、padding-box、border-box、margin-box。

所以如何使用这些属性来进行居中垂直呢？

## 2.1 padding填充

```html
<div class='wrap'>
  <div class='content'></div>
</div>
```

```less
@wrapWidth: 400px;

.wrap{
  //margin设定auto，左右自动空出相同大小的空间
  margin-left: auto;
  margin-right: auto;
  margin-top: 20px;
  width: @wrapWidth;
  height: @wrapWidth;
  background-color: #ccc;
}

.content{
  @contentWidth: 100px;
  width: @contentWidth;
  height: @contentWidth;
  padding: (@wrapWidth - @contentWidth) / 2;//左右各150px
  background-color: #333;
  background-clip: content-box;
}
```

 ##2.2 margin填充
 使用margin进行居中填充的缺点是，必须要预先知道元素的宽度，这点不够灵活。

```html
<div class='wrap'>
  <div class='content'></div>
</div>
```

```less
.wrap{
  @wrapHeight: 400px;
  @contentHeight: 100px;
  overflow: hidden;
  width: 100%;
  height: @wrapHeight;
  background-color: #ccc;
  .content{
    margin-left: auto;
    margin-right: auto;
    margin-top: (@wrapHeight - @contentHeight) / 2;
    width: 100px;//必须知道具体值！！！！
    background-color: #333;
    color: #fff;
  }
}
```

##2.3 absolute布局上下文水平垂直居中
实现原理为，通过absolute布局left到**父容器**的中点即50%。然后再将子容器向左**移动自身宽度**的50%。

```html
<div class="wrap">  
  <div class="ele margin">水平垂直居中水平垂直<br>居中水平垂直居中水平<br>垂直居中水平垂直居<br>中水平垂直居中</div>
</div>

<div class="wrap">  
  <div class="ele translate">水平垂直居中水平垂直<br>居中水平垂直居中水平<br>垂直居中水平垂直居<br>中水平垂直居中</div>
</div>

<div class="wrap">  
  <div class="ele relative">
    <div class="ele-inner">水平垂直居中水平垂直<br>居中水平垂直居中水平<br>垂直居中水平垂直居<br>中水平垂直居中</div>
  </div>
</div>  
```

```less
.wrap{
  position: relative;
  width: 100%;
  height:200px;
  border: 1px solid;
  background-color: #ccc;
  .ele{
    position: absolute;
    left: 50%;
    top: 50%;
    background-color: #333;
    &.margin{
      width: 160px;
      height: 100px;
      margin-left: -80px;//向左移动自身宽度的50%
      margin-top: -50px;//向上移动自身高度的50%
    }
    &.translate{
      transform: translate3d(-50%, -50%, 0);
    }
    .ele-inner{
      position: relative;
      left: -50%;
      top: -50%;
      width: 100%;
      height: 100%;
      background-color: #333;
    }
    &.relative{
      width: 150px;
      height: 100px;
      background-color: transparent;
    }
  }
}
```

###text-align:center + absolute
text-align:center作用的是文本而不是absolute元素，但是当absolute元素为**inline-block**的时候，_**它会受到文本的影响。**_

**你可能会奇怪，为什么明明没有文本元素`text-align`依然有效？其实，其中是包含着隐藏的文本节点。**

```html
<div class='wrap'>
  <div class='ele'></div>
</div>
```

```less
.wrap{
  text-align: center;
  
  width: 100%;
  height: 400px;
  background-color: #ccc;
  font-size: 0;
}
.ele{
  position: absolute;
  margin-left: -(100px / 2);//向左拉回自身的50%
  margin-top: -(400px - 100px) / 2;
  
  width: 100px;
  height: 100px;
  display: inline-block; //强调！！display属性要inline-block才会对text-align有反应。
  background-color: #333;
}
```

###absolute+margin:auto

```html
<div class='wrap'>
  <div class='ele'>
  </div>
</div>
```

```less
html, body{
  width: 100%;
  height: 100%;
  margin: 0;
}

.wrap{
  position: relative;
  width: 100%;
  height: 100%;
  background-color: #ccc;
  .ele{
    position: absolute;
    left: 0;
    top: 0;
    bottom: 0;
    margin: auto;
    width: 100px;
    height: 100px;
    background-color: #333;
  }
}
```

#3. float布局水平垂直居中
使用这种方法

```html
<div class='wrap'>
  <div class='ele'>
    <div class='ele-inner'>
      居中居中居中居中居中居中居中居中居中居中居中居中居中居中居中居中居中居中居中居中居中居中居中居中居中居中居中居中居中
    </div>
    
  </div>
</div>
```

```js
.wrap{
  float: left;
  width: 100%;
  height: 400px;
  background-color: #ccc;
  .ele{
    float: left;
    position: relative;
    left: 50%;
    top: 50%;
  }
  .ele-inner{
    position: relative;
    left: -50%;
    transform: translate3d(0, -50%, 0);
    background-color: #333;
    color: #fff;
  }
}
```

#4. 图片居中方法
其实就是添加两个相同的`img`图片，一个`visibility`属性为**hidden**用于占位，另一个正常显示。

```html
<div class="wrap">  
  <p>
    <img src="http://nec.netease.com/img/s/1.jpg" alt="" />
    <img src="http://nec.netease.com/img/s/1.jpg" alt="" />
  </p>  
</div> 
```

```scss
html, body{
  width: 100%;
  height: 100%;
  margin: 0;
  .wrap{
    position: relative;
    width: 100%;
    height: 100%;
    p{
      position: absolute;
      left: 50%;
      top: 50%;
    }
    img{
      &:nth-child(1){
        position: static;
        visibility: hidden;
      }
      &:nth-child(2){
        position: absolute;
        right: 50%;
        bottom: 50%;
      }
    }
  }
}
```

#5 margin-bottom: -50%
其实就是先用一个占位元素*placeholder*，占据50%的高度。然后再给定`margin-bottom`为-50%，这样做到让*content*垂直居中，然后再通过`transform:translate3d`来进行水平居中。这种方法有一定的缺点，需要固定死父容器和`margin-bottom`的高度。不过优点是可以兼容IE的低版本。

```html
<div class='wrap'>
  <div class='placeholder'></div>
  <div class='content'></div>
</div>
```

```scss
.wrap{
  float: left;
  width: 100%;
  height: 400px;
  background-color: #ccc;
  @contentHeight: 100px;
    .placeholer{
      float: left;
      width: 100%;
      height: 50%;
      margin-bottom: -(@contentHeight / 2);
    }
    .content{
      position: relative;
      left: 50%;
      transform: translate3d: (-50%, 0, 0);
      clear: both;
      max-width: 100px;
      height: @contentHeight;
      background-color: #333;
    }
}
```

#6 IFC布局水平垂直居中
**IFC介绍：** http://www.cnblogs.com/piaopiao7891/p/3665990.html
##6.1 `text-align:center + vertical-align: middle`

```html
<div class="wrap">  
  <div class='placeholder'><!--占位元素，用来作为居中元素的参照物--></div>
  <div class="ele"></div>
</div>  
```

```scss
.wrap {
  width: 100%;
  height: 400px;
  
  text-align: center;
  font-size: 0;
  background-color: #ccc;
  .placeholder
  .ele  {
    vertical-align: middle;
    display: inline-block;
  }
  .placeholder{
    overflow: hidden;
    width: 0;
    min-height: inherit;
    height: inherit;
  }
  .ele{
    width: 100%;
    height: 100px;
    background-color: #333;
  }
}
```

##6.2 `text-align: center + line-height`

```html
<div class="wrap">  
  <div class="ele">居中居中居中居中居中居中<br>居中居中居中居中居中居中居中<br>居中居中居中居中居中居中居中居中<br>居中居中居中居中居中居中居中居中</div>
</div>  

```

```scss
.wrap{
  text-align: center;
  line-height: 400px;
  
  width: 100%;
  height: 400px;
  background-color: #ccc;
  font-size: 0;
  .ele{
    line-height: normal;
    vertical-align: middle;
    display: inline-block;
    background-color: #333;
    font-size: 18px;
    color: #fff;
  }
}
```

#7 flex布局

flex默认为横向布局

```html
<div class='wrap'>
  <div class='ele'>
    居中居中居中居中居中居中居中
  </div>
</div>
```

```scss
.wrap{
  display: flex;
  align-item: center;
  justify-content: center;
}
```

资料：http://f2e.souche.com/blog/jie-du-cssbu-ju-zhi-shui-ping-chui-zhi-ju-zhong/