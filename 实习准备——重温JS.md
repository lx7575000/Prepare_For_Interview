#location对象
location对象用于保存当前标签页的URL信息。
##查询URL中字符串参数

```js
例子URL的 www.test.com?type=music&loc=ningbo
function getQueryStringArgs(){
  //查询是否有query的参数，如果有就去掉其中的？后返回后续的具体内容 返回type=music&loc=ningbo
  let qs = location.search.length > 0 ? location.search.substring(1) ? '';
  //判断是否有query参数，将各个参数项分开为type=music和loc=ningbo两部分
  let items = qs.length ? qs.split("&") : [];
  let args = {};
  let name = null, value = null, item = null;
  for(let i = 0; i < items.length; i++){
    //分别将各个query项进行键值分离为type和music、loc和ningbo
    item = items[i].split('=');
    //应用了解构
    [name, value] = item;
    
    //保险起见再确认键值是否存在
    if(name.length > 0){
      args[name] = value;
    }
    return args;
  }
}
```

#事件代理和委托
##事件流
事件流分为两类，IE6的冒泡事件流和非IE6的捕获事件流。
##IE冒泡事件流
冒泡的这种相对简单一些，就是子元素触发一个特定事件，然后这个事件发生后会不怕事大的一直向上层父组件进行传递广播，最终传递到HTML标签的document为止。
##捕获事件流
与冒泡相比，捕获事件流相对就多了些内容。触发事件的这个子元素它是整个事件传播过程的中点，始发于document（html标签）对象，经过一系列父组件到达惹事的子元素标签处，然后就原路返回。
所以整个捕获事件流可以分为三个部分：
>  捕获阶段 --> 目标阶段(在触发事件的节点上，会发生两次) -->冒泡阶段

#事件委托
我对于事件委托的理解就是通过使用捕获事件流机制，将原本要放在子节点身上的事情一股脑的扔给他爹。然后在子节点犯事的时候，他爹通过去向下追寻他时候顺带把我们需要子节点要完成的事情告诉他，然后该子节点会在目标阶段的**第二次**事件流时触发委托事件。
下面给出个例子代码：

```js
let ul = document.querySelector('.nav');

ul.addEventListener('click', (e)=>{
  let target = e.target;
  if(target&&target.nodeName.toUpper() === 'LI'){
    alert(`${target.className} has been clicked ..`);
  }
});
```

```html
<ul class='nav'>
  <li class='home'>Home</li>
  <li class='pages'>Pages</li>
  <li class='history'>History</li>
  <li class='about'>AboutMe</li>
</ul>
```

#JS Hositing
链接： https://segmentfault.com/a/1190000004345355
这篇讲的比较好

#null 和 undefined
##理论区别（然并卵）
null是一个表示无的**对象**，当其被转为数值对象时为**0**。
undefined是一个表示无的**原始值**，转为数值时为**NaN**
##实际用法
null表示没有对象，即该处不应该有值。
  1. 作为函数参数，表示该函数的参数不是对象。
  2. 作为对象原型链的重点---Object.prototype = null (一个空对象)
  
undefined表示缺少值，即此处可以有值，但还没有定义
  1. 变量被声明了，但没有赋值时，就等于undefined。
  2.  调用函数时，应该提供的参数没有提供，该参数等于undefined。
  3. 对象没有赋值的属性，该属性的值为undefined。
  4. 函数没有返回值时，默认返回undefined。

#关于JavaScript中的**event loop**