# link与@import的区别
**差别1**：老祖宗的差别。link属于XHTML标签，而@import完全是CSS提供的一种方式。

link标签除了可以加载CSS外，还可以做很多其它的事情，比如定义RSS，定义rel连接属性等，@import就只能加载CSS了。

**差别2**：加载顺序的差别。当一个页面被加载的时候（就是被浏览者浏览的时候），link引用的CSS会同时被加载，而@import引用的CSS会等到页面全部被下载完再被加载。所以有时候浏览@import加载CSS的页面时开始会没有样式（就是闪烁），网速慢的时候还挺明显。

**差别3**：兼容性的差别（其实问题不大了）。由于@import是CSS2.1提出的所以老的浏览器不支持，@import只有在IE5以上的才能识别，而link标签无此问题。

**差别4**：使用dom控制样式时的差别。当使用javascript控制dom去改变样式的时候，只能使用link标签，因为@import不是dom可以控制的。

**差别5**：@import可以在css中再次引入其他样式表，比如可以创建一个主样式表，在主样式表中再引入其他的样式表

#浏览器是怎么对HTML5的离线储存资源进行管理和加载的呢？
[有趣的HTML5：离线存储](https://segmentfault.com/a/1190000000732617)

在html标签中添加`manifest='chche.manifest'`

```html
<!DOCTYPE HTML>
<html manifest = "cache.manifest">
...
</html>
```

然后你可以在cache.manifest中添加需要缓存的文件内容

```html
//CACHE MANIFEST
CACHE:
//需要离线存储的资源列表
js/app.js
css/style.css

NETWORK:
//在线访问资源
resourse/logo.png

FALLBACK:
/*
表示如果访问第一个资源失败，那么就使用第二个资源来替换他，比如下面这个文件表示的就是如果访问根目录下任何一个资源失败了，那么就去访问offline.html。
*/

/ /offline.html
```

#详说 Cookie, LocalStorage 与 SessionStorage
##Cookie
主要用于传递登陆信息，由于通常只有4k大小，所以不建议也不应该在其中加入过多信息。
##localStorage
HTML5中新加入的技术，虽然在IE6时已经实现类似功能。特点是可以长时间（永久）存储一定信息，电商网站可以用于存储购物车内物品的信息，在线游戏可以用来存储游戏相关内容。
##sessionStorage
与`localStorage`类似，同样是HTML5新加入的技术。不过与前者不同，sessionStorage中存储内容的生存周期是有限的。刷新页面内容不会消失，但是关闭页面内容就会被清空掉。
##Difference
###Cookie
Cookie的大小通常为4K左右，会被放在HTTP报头中，在服务器和浏览器端来回传递。
###localStorage和sessionStorage
通常有5M大小，仅仅保存在浏览器端，不会与服务器端通信。原生接口比较好，方便使用。
##应用场景
Cookie：老老实实用来做用户登录认证
localStorage：存储购物车内信息，HTML5游戏存储本地数据
sessionStorage：注册表单，存储多步骤表单填写内容，然后最后将总内容一起提交。

#Label的作用是什么？是怎么用的？
##与输入框配合
通常使用在`label`标签中添加**for**属性，用于和对应的`input`标签搭配使用。`input`标签内的id/name属性与`label`中的**for**对应。

```html
<label for='username' >UserName</label>
<input name='username' type='text'/>
<label for='email' >UserName</label>
<input id='email' type='email'/>
```
如上述代码，此时点击`label`的UserName内容就会自动**focus**到input标签上。

##与单选框配合
```html
<label for='ck1'>JS </label>
<input type='checkbox' id='ck1' value='JavaScript'/>
<label for='ck2'>CSS </label>
<input type='checkbox' id='ck2' value='CSS'/>
```
恩，上述代码的效果。你懂的。