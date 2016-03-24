#1. 在JS中`undefined`和`not defined`的区别
在JS中，如果想要使用一个不存在的变量，JS会报错提醒你，这个变量是"**not defined**"，并且会停止执行代码。然而，如果你使用`typeof undeclared_variable`，系统会返回`undefined`。

在更加深入学习之前，我们需要需要了解**定义**和**声明**的不同。

`var x`是对变量的声明，因为你没有为其定义具体值。但是你提醒了系统它的存在，并且需要对其分配内存空间。

```JavaScript
var x //声明 变量 x
console.log(x) // 输出 : undefined
```

`var x = 1`此处对变量先后进行了声明和定义。首先引擎会对变量x解析为先声明，然后对其赋值。在JavaScript中，无论是变量还是函数，**声明**都会被**提升(hoisting)**到其作用域的最顶部。

赋值行为会按代码顺序执行，因此当我们想要使用一个变量但是没有被定义时，我们会得到`undefined`。
```JavaScript
var x //声明
if(typefo x === 'undefined') //会返回true

假如没有对变量进行声明和定义，此时我们使用该变量会返回"not defined"的结果

console.log(y) // 输出： ReferenceError: y is not defined
```

#2. 下面会输出什么？
```JavaScript
 var y = 1;
  if (function f(){}) {
    y += typeof f;
  }
  console.log(y);
```

上述代码的输出是"1undefined"，if条件语句使用`eval`进行计算，因此eval(function f(){}) 会返回**`function f(){}`**。因此，会执行条件语句内的表达式---`typeof f`并返回`undefined`。这是因为if条件判断中的代码只会在代码运行时进行计算，**但是**if条件语句的下面的执行代码在运行之间就已经计算完毕了。

```JavaScript
 var k = 1;
  if (1) {
    eval(function foo(){});
    k += typeof foo;
  }
  console.log(k); 
```
当然，上面的代码还是会输出`"1undefined"`
只有下面的代码才会执行出我们想要的结果。
```JavaScript
 var k = 1;
  if (1) {
    function foo(){};
    k += typeof foo;
  }
  console.log(k); // output 1function
```

#3. JavaScript中创建私有方法的缺点

缺点很明显，这么做非常浪费内存。每次拷贝创建新对象都会复制一份方法。

```JavaScript
var Employee = function (name, company, salary) {
    this.name = name || "";       //Public attribute default value is null
    this.company = company || ""; //Public attribute default value is null
    this.salary = salary || 5000; //Public attribute default value is null

    // Private method
    var increaseSalary = function () {
        this.salary = this.salary + 1000;
    };

    // Public method
    this.dispalyIncreasedSalary = function() {
        increaseSlary();
        console.log(this.salary);
    };
};

// Create Employee class object
var emp1 = new Employee("John","Pluto",3000);
// Create Employee class object
var emp2 = new Employee("Merry","Pluto",2000);
// Create Employee class object
var emp3 = new Employee("Ren","Pluto",2500);
```

如上述代码，每个变量`emp1, emp2, emp3`都各自拥有自己的`increaseSalary`私有方法。
所以，衷心建议你在不必要的情况下不要使用私有方法。

#4. JavaScript中的闭包是什么，举个栗子。

闭包就是一个函数方法被定义在另一个函数方法（父函数）当中。并且可以获得并使用在父函数中定义的变量。

闭包可以获得三个作用域中的变量。
* 声明在其内部的变量。
* 声明在父函数中的变量。
* 声明在全局作用域中的变量。

```JavaScript
// Parent self invoking function 
(function outerFunction (outerArg) { // begin of scope outerFunction
    // Variable declared in outerFunction function scope 
    var outerFuncVar = 'x';    
    // Closure self-invoking function 
    (function innerFunction (innerArg) { // begin of scope innerFunction
        // variable declared in innerFunction function scope
        var innerFuncVar = "y"; 
        console.log(          
            "outerArg = " + outerArg + "\n" +
            "outerFuncVar = " + outerFuncVar + "\n" +
            "innerArg = " + innerArg + "\n" +
            "innerFuncVar = " + innerFuncVar + "\n" +
            "globalVar = " + globalVar);

    }// end of scope innerFunction)(5); // Pass 5 as parameter 
}// end of scope outerFunction )(7); // Pass 7 as parameter 
```

上述代码中，`innerFunction`就是被定义在`outerFunction`中的闭包函数。它可以获得所有定义在`outerFunction`作用域中的变量。除此之外，定义在函数中的闭包函数方法也可以获得定义在全局作用域当中的变量。
所以，上述代码的输出如下：

```JavaScript
  outerArg = 7
  outerFuncVar = x
  innerArg = 5
  innerFuncVar = y
  globalVar = abc
```

#5. 写一个`mul`函数，当执行时会产生下列输出结果

```JavaScript 
console.log(mul(2)(3)(4)) //output 24
console.log(mul(4)(3)(4)); // output : 48
```
下面的答案中会解释它如何工作：
```JavaScript
function mul(x){
  return function(y){
    return function(z){
      return x * y * z;
    }
  }
}
```

`mul`函数接受第一个参数，并且会返回一个匿名函数，它会接受第二个参数。同理，会返回接收第三个参数的匿名函数。最后它会返回上述三个参数的相乘结果。

在JavaScript中，一个函数当中定义的内部函数可以接收外部函数的参数。因此，在JavaScript中函数是一级公民，可以被其他函数返回，并且接收它的参数使用。
* 一个函数是对象的一个实例
* 一个函数可以拥有属性，并且链接返回它的`constructor`方法。
* 函数方法可以被声明赋值给一个变量
* 函数方法可以被当做参数传递
* 函数可以被当做另一个函数的返回值

#6. 在JavaScript中如何清空一个数组
例如：
`var arrayList =  ['a','b','c','d','e','f'];`
##**我们要如何清空它？**
###方法一
`arrayList = []`

上述代码会将变量`arrayList`赋值为新的空数组。假如你的原生数组**没有被引用**的话，推荐你这么做。因为这样会重新创建一个新的空数组。当然，你这么做需要非常小心。因为假如你不小心将这个数组赋值引用给其他变量，则上面的做法不会令其被清除，导致内存浪费。
**栗子：**
```JavaScript
var arrayList = ['a','b','c','d','e','f']; // Created array 
var anotherArrayList = arrayList;  // Referenced arrayList by another variable 
arrayList = []; // Empty the array 
console.log(anotherArrayList); // Output ['a','b','c','d','e','f']
```

###方法二
`arrayList.length = 0`
上述代码会通过重新设定数组长度为0，从而清除掉存在的数组。这个方法同样可以清除掉引用变量中原生数组的值。因此，这个方法最有效。
**栗子：**

```JavaScript
var arrayList = ['a','b','c','d','e','f']; // Created array 
var anotherArrayList = arrayList;  // Referenced arrayList by another variable 
arrayList.length = 0; // Empty the array by setting length to 0
console.log(anotherArrayList); // Output []
```

###方法三
`arrayList.splice(0, arrayList.length)`
执行上面代码同样能完成要求，这个方法同样能够同时更新掉引用变量中的数组内容。
```JavaScript
var arrayList = ['a','b','c','d','e','f']; // Created array 
var anotherArrayList = arrayList;  // Referenced arrayList by another variable 
arrayList.splice(0, arrayList.length); // Empty the array by setting length to 0
console.log(anotherArrayList); // Output []
```

###方法四
```JavaScript
while(arrayList.length){
    arrayList.pop();
}
```
上述代码方法一看就知道能完成任务，但我相信你绝对不会用这种麻烦方式的。

#7. 如何判断一个对象是否为数组？
判别一个对象是否是一个特殊类的实例的最好方式是使用`Object.prototype`的`toString`方法
`var arrayList = [1, 2, 3];`
最好的例子就是当我们对JavaScript方法进行重载。例如，我们有一个方法`great`，它可以接收一个或一组字符串。为了令我们的方法适应多种环境，我们需要知道参数的类型。
```JavaScript
 function greet(param){
     if(){ // here have to check whether param is array or not 
     }else{
     }
 }
```
然而，上述执行代码并不一定必需检测是否为数组类型。我们可以直接查看是否是字符串类型，在`else`语句中使用数组类型的逻辑方法。
```JavaScript
 function greet(param){
     if(typeof param === 'string'){ 
     }else{
       // If param is of type array then this block of code would execute
     }
 }

```
现在，我们可以使用上述提到的两种执行方法。但是，当我需要加入参数为`object`情况时，我们又苦逼的遇到麻烦了。
如上文般重新检测对象类型。我们可以使用`Object.prototype.toString`

```JavaScript
if( Object.prototype.toString.call( arrayList ) === '[object Array]' ) {
    console.log('Array!');
}
```
假如你使用`JQuery`，你也可以使用`JQuery`自带的`isArray`方法
```JavaScript
 if($.isArray(arrayList)){
    console.log('Array');
  }else{
      console.log('Not an array');
  }
```

在现代浏览器当中，我们
可以直接使用
`Array.isArray(arrayList);`
它被**Chrome 5, Firefox 4.0, IE 9, Opera 10.5 and Safari 5**这些浏览器所支持。

#8. 一问：下面代码会输出什么？
```JavaScript
var output = (function(x){
    delete x;
    return x;
  })(0);

  console.log(output);
```

上面代码输出为0，`delete`操作符通常被用于删除对象属性。此处的`x`不是对象，是局部变量。因此`delete`操作符不会对本地变量产生影响。

#9.二问：下面这个代码又会输出什么？
```JavaScript
var x = 1;
var output = (function(){
    delete x;
    return x;
  })();

  console.log(output);
```

上面代码会输出的是**1**，`delete`操作符通常被用于删除对象属性。此处x不是对象，它是一个全局的数值变量。



#10. 三问：下面代码会输出什么？
```JavaScript
var x = { foo : 1};
var output = (function(){
    delete x.foo;
    return x.foo;
  })();

  console.log(output);
```

输出的是**undefined**，`delete`操作符通常被用于删除对象属性。此处的x真的是一个对象了，并且具有属性`foo`。并且作为立即运行的函数方法，它会直接删除x的foo属性，此后当我们想要引用返回被删除属性，所得到结果只会是**`undefined`**。

#11. 四问：下面代码会输出啥？
```JavaScript
var Employee = {
  company: 'xyz'
}
var emp1 = Object.create(Employee);
delete emp1.company
console.log(emp1.company);
```

上述代码会输出xyz,此处的`emp1`对象所使用的`company`属性是属于其原型链上的属性，`delete`操作符不能删除原型链上的属性。
**emp1**对象本身并不存在**company**属性，你可以使用`console.log(emp1.hasOwnProperty('company')) // false`来进行测试。当然虽然在**emp1**上我们无法做到，但是我们可以通过直接删除**Employee**上的**company**属性来达成目的。或者也可以通过删除`emp1`对象的**`__proto__`**属性上的内容。`delete emp1.__proto__.company`