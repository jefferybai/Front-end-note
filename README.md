# Front-end-note
## js基础知识点
- [js中的方法](#方法)
- [js中的变量](#变量)
- [js中比较两个值是否相当](#js中比较两个值是否相当)
- [js中的new运算符](#js中的new运算符)

- [闭包](#闭包)
- [高阶函数](#高阶函数)
- [柯里化](#柯里化)

## js设计模式
- [工厂模式](#工厂模式)


## 方法
javascript的方法可以分为三类：

1. 类方法

2. 对象方法

3. 原型方法

For example:

##### `People function`

```js
function People(name)
{
  this.name=name;
  //对象方法
  this.introduce=function(){
    console.log("My name is "+this.name);
  }
}

//类方法
People.Run=function(){
  console.log("I can run");
}

//类方法this存储变量
People.getInstance = function() {
  if(!this.instance) {
    this.instance = new People();
  }
  return this.instance
}

//原型方法
People.prototype.introduceChinese=function(){
  console.log("我的名字是"+this.name);
}

测试
let people = new People('jeffery.bai');
people.introduce(); // My name is jeffery.bai
people.run(); //function is not define
People.run(); //I can run
people.introduceChinese()//我的名字是jeffery.bai

```

## 变量
js中的变量分两种全局变量局部变量

1.全局变量：声明在函数外部的变量（所有没有var直接赋值的变量都属于全局变量）

2.局部变量：声明在函数内部的变量

变量的作用域有三种

1，全局 2，函数 3，块级

```js
windowScope = '给window的windowScope赋值';
console.log(scope) //undefine （声明未赋值输出，  作用域提升）
var scope = "全局变量";
function checkscope(){
    var scope = "";
    function nested(){
        let blockScope = '块级变量';
        scope = "函数变量";
    }
    nested();
    console.log(scope);//函数变量
    console.log(blockScope) //blockScope is not defined
}
console.log(scope);//全局变量
console.log(window.windowScope);//给window的windowScope赋值
checkscope();
```

## js中比较两个值是否相当

两个普通字符串/数字时， == 操作符会先转换类型，然后在做比较， === 不会转换类型

两个对象时，JavaScript会比较其内部引用，当且仅当他们的引用指向内存中的相同对象（区域）时才相等，即他们在栈内存中的引用地址相同。

## js中的new运算符

JavaScript中没有类的概念，JavaScript 中的根对象是 Object.prototype 对象。Object.prototype 对象是一个空的 对象。

我们在 JavaScript 遇到的每个对象，实际上都是从 Object.prototype 对象克隆而来的， Object.prototype 对象就是它们的原型。

所以，js中要得到一个对象，不是通过实例化类，而是找到一个对象作为原型并克隆它

```js
var objectFactory = function(){
  var obj = new Object(), // 从 Object.prototype 上克隆一个空的对象
  Constructor = [].shift.call( arguments ); // 取得外部传入的构造器，此例是 Person
  obj.__proto__ = Constructor.prototype; // 继承传入的原型
  var ret = Constructor.apply( obj, arguments ); //通过构造函数给对象赋予初始值
  return typeof ret === 'object' ? ret : obj;
};

function Person( name ){ 
  this.name = name;
};

Person.prototype.getName = function(){ 
  return this.name;
};

var a = objectFactory( Person, 'jeffery.bai' );
console.log( a.name ); // 输出:jeffery.bai
console.log( a.getName() ); // 输出:jeffery.bai

```



## 闭包

闭包是一个能读取函数内部变量的子函数，函数与外界连接的桥梁， 一个未释放内存的栈区

闭包的构成：引用了一个函数返回的函数

闭包的用途：存储变量， 保证变量不会被垃圾回收器回收

注意： 使用过多的闭包容易造成内存泄漏

 ```js
 　　function f1(){
　　　　var n = 999;
　　　　nAdd = function() { n += 1}
　　　　function f2(){
　　　　　　console.log(n);
　　　　}
　　　　return f2;
　　}
　　var result = f1(); //闭包的形成，存储局部变量
　　result(); // 999
　　nAdd();
　　result(); // 1000
  ```
  
  ## 高阶函数
  
  高阶函数是指至少满足下列条件之一的函数。
  
  1，函数可以作为参数被传递; 2， 函数可以作为返回值输出。
  
  
  
 ## 柯里化
参数不够用就存储之前的参数，返回一个函数，继续接受参数， 直到参数够用为止再执行这个函数。

又称部分求值（Partial Evaluation），是把接受多个参数的函数变换成接受一个单一参数（最初函数的第一个参数）的函数，并且返回接受余下的参数而且返回结果的新函数的技术.

 #### 常见作用：1,延迟计算/运行 2. 动态创建函数 3. 参数复用


1. 延迟计算下面是一个部分求和的例子：
```js
function currying(func) {
    const args = [];
    return function result(...rest) {
        if (rest.length === 0)
            return func(...args);

        args.push(...rest);
        return result;
    }
}

const add = (...args) => args.reduce((a, b) => a + b);

const sum = currying(add);

sum(1,2)(3);
sum(4);
sum(); // 10
```
2. 动态创建函数例如兼容现代浏览器和IE浏览器的添加事件方法，我们通常会这样写：
```js
const addEvent = function (elem, type, fn, cature) {
    if (window.addEventListener) {
        elem.addEventListener(type, (e) => fn.call(elem, e), capture);
    } else if (window.attachEvent) {
        elem.attachEvent('on' + type, (e) => fn.call(elem, e);
    }
}
```
这种方法显然有个问题，就是每次添加事件处理都要执行一遍`if {...} else if {...}`。其实用下面的方法只需判断一次即可：
```js
const addEvent = (function () {
    if (window.addEventListener) {
        return (elem, type, fn, capture) => {
            elem.addEventListener(type, (e) => fn.call(elem, e), capture);
        };
    } else {
        return (elem, type, fn, capture) => {
            elem.attachEvent('on' + type, (e) => fn.call(elem, e);
        };
    }
})();
```
这个例子，第一次`if {...} else if {...}`判断之后，完成了部分计算，动态创建新的函数来处理后面传入的参数，以后就不必重新进行计算了。这是一个典型的柯里化的应用。

3. 参数复用当多次调用同一个函数，并且传递的参数绝大多数是相同的时候，那么该函数就是一个很好的柯里化候选。例如我们经常会用`Function.prototype.bind`方法来解决上述问题。
```js
const obj = { name: 'test' };
const foo = function (prefix, suffix) {
    console.log(prefix + this.name + suffix);
}.bind(obj, 'currying-');
foo('-function'); // currying-test-function
```
与`call`/`apply`方法直接执行不同，`bind`方法将第一个参数设置为函数执行的上下文，其他参数依次传递给调用方法（函数的主体本身不执行，可以看成是延迟执行），并动态创建返回一个新的函数。这很符合柯里化的特征。下面来手动实现一下`bind`方法：
```js
Function.prototype.bind = function (context, ...args) {
    return () => this.call(context, ...args);
};
```



