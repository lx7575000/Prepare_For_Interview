```html
HTML结构

        <div class="box">
            <div class="box-top">
              <!--设定上边，各div 标签1px-->
                <div class="bd bd-1"></div>
                <div class="bd bd-2"></div>
                <div class="bd bd-3"></div>
                <div class="bd bd-4"></div>
                <div class="bd bd-5"></div>
            </div>
            <div class="box-cnt"></div>
            <div class="box-bot">
              <!--设定下边，各div 标签1px-->
                <div class="bd bd-5"></div>
                <div class="bd bd-4"></div>
                <div class="bd bd-3"></div>
                <div class="bd bd-2"></div>
                <div class="bd bd-1"></div>
            </div>
        </div>

```

```css
CSS

.box {
  //最外层设定整个矩形的宽度
  width: 50%;
}

.box-cnt {
  //用于设定矩形高度
  height: 150px;
  border-left: 5px solid #000;
  border-right: 5px solid #000;
}

.bd {
  //设定上下边的厚度，长度自适应，基于.box中的宽度设定
  height: 1px;
  font-size: 0;
  background-color: #000;
}

.bd-1 {
  //由于宽度一定，所以左右分别空3像素
  margin: 0 3px;
}
.bd-2 {
//左右相对差两像素
  margin: 0 2px;
}
.bd-3 {
  margin: 0 1px;
}
.bd-4 {
  margin: 0 1px;
}

```