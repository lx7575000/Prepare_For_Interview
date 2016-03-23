```html
var a = 5; function test(){     var a = a + 5;    console.log(a);    console.log(this.a);    return a;}console.log(a);test();console.log(test()+10);
```

考点：hoist，作用域，this，undefined + 数值的结果
上述代码可以变为：

```js
var a = 5;
function text(){
  var a = undefined; //作用域提升hoist
  a = a + 5; //undefined + 5 ==> NaN
  console.log(a) // => NaN
  console.log(this.a) // => 5 执行作用域是全局，所以this === window
  return a; // => NaN
}
console.log(a) // 5
test() // => NaN
console.log(test() + 10 ) // => NaN

/*整个执行过程*/
5
NaN
5
NaN
5
NaN
```