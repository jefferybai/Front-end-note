# Front-end-note
## js基础知识点
- [js中的方法](#方法)
- [js中的变量](#变量)
- [js中比较两个值是否相当](#js中比较两个值是否相当)
- [js中的new运算符](#js中的new运算符)
- [this](#this)
- [继承](#继承)
- [闭包](#闭包)
- [高阶函数](#高阶函数)
- [柯里化](#柯里化)
- [compose函数](#compose函数)

## [js设计模式](#设计模式)
- [单例模式](#单例模式)

- [工厂模式](#工厂模式)
- [观察者模式](#观察者模式)



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
## this

this 既不是函数自身的引用，也不是函数词法作用域的引用

this 实际上是在函数被调用时建立的一个绑定，它指向什么是完全由函数被调用的调用点来决定的。

## 继承

每个对象的实例都是自己构造函数的prototype， 如果对象无法响应么个请求那么会通过_proto_查找自己的原型链, _proto_指向创建它的方法对象(构造函数)的原型对象

```js
function Animal() {
  this.age = 0;
}

Animal.prototype.getName = function() {
  console.log("animal")
}

class Dog extend Animal {

  constructor() {
    super()
  }
  
  sound() {
    console.log('wang wang wang')
  }
}

let dog = new Dog();
dog.getName() // 'animal'

```



## 闭包

闭包就是函数能够记住并访问它的词法作用域，即使当这个函数在它的词法作用域之外执行时。

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
  
   ```js
   //函数作为参数被传递
   [1, 2, 3].filter((item) => item > 2);
   
    
    //函数可以作为返回值输出
    var isType = function( type ){ 
      return function( obj ){
        return Object.prototype.toString.call( obj ) === '[object '+ type +']'; 
      }
    };
    var isString = isType( 'String' ); var isArray = isType( 'Array' ); 
    var isNumber = isType( 'Number' );
    console.log( isArray( [ 1, 2, 3 ] ) ); // true
  ```
  
  
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

## compose函数
compose是一个装饰者模式，它的作用是动态的组合柯里化以后的函数，赋予函数新的职责和功能，最典型的例子就是redux的middelware

作用：
compose(funcA, funcB, funcC) // （）=> funcA(funcB(funcC()))

返回值： (Function): 从右到左把接收到的函数合成后的最终函数

基本实现： 

```js
var compose = function(f,g) {
  return function(x) {
    return f(g(x));
  };
};
```

具体实现： 

```js
const compose = (arr) => {
  return function(ctx) {
    [...arr].reverse().reduce((func, item) => {
      return function(ctx) {
        item(ctx, function() {
          func(ctx)
        })
      }
    }, ()=>{})(ctx)
  }
}
```

## 设计模式

设计模式（Design Pattern）是一套被反复使用、多数人知晓的、经过分类的、代码设计经验的总结。

使用设计模式的目的：为了代码可重用性、让代码更容易被他人理解、保证代码可靠性。 设计模式使代码编写真正工程化；

总结了一些设计原则具体如下：

1,封装变化:找出应用中可能变化需要变化之处,把他们独立出来,不要和那些不需要变化之处的代码混在一起.
 
2,针对接口编程

3,多用组合少用继承

4,为了交互对象之间的松耦合设计而努力!

5,类应该扩展开放,对修改关闭

6,要依赖抽象,不要依赖具体类

7,最少知识原则:只和你的密友谈话

8,别打电话给(调用)我,我会打电话给(调用)你

9,一个类应该只有一个引起变化的原因

而根据这些准则，则衍生出了如下设计模式。
 

## 单例模式
js中的单例模式是指，js中的么个对象在一个程序中只能被克隆一次

简单单例模式：
```js
 function Singleton() {}
 
 Singleton.getInstance = function() {
  if(!this.instance) {
    this.instance = new Singleton();
  }
  return this.instance;
 }
 ```
 
## 工厂模式
作用： 为调用者和被调用者之间的松耦合设计, 依赖接口，而不依赖具体对象。

比如你需要一名程序员， 而人才市场有各种各样的人才， 你只需要向招聘网站发送相应的人才类型，招聘网站就能给你推送来你想要的简历。而这个人的籍贯，性别，年龄，身高，兴趣则不是你所关心的。

1.隐藏具体类名，很多类隐藏得很深的，而且可能会在后续版本换掉 （兴趣）

2.避免你辛苦的准备构造方法的参数（怎么来的）

3.这个工厂类可以被配置成其它类 （传递参数切换各种不同的学历的人）


## 观察者模式
作用： 观察者模式定义了一对多的依赖关系，将主题和观察者之间的耦合转移到模式上面并让模式去维护两者之间的依赖关系。
实现：
```js
var event = {};    //发布者
event.clietList = []; //发布者的缓存列表（应聘者列表）

event.listen = function(fn) { //增加订阅者函数
    this.clietList.push(fn);
};

event.trigger = function() { //发布消息函数
    for (var i = 0; i < this.clietList.length; i++) {
        var fn = this.clietList[i];
        fn.apply(this, arguments);
    }
};

event.listen(function(time) { //某人订阅了这个消息
    console.log('message：' + time);
});

event.trigger('2016/10',yes); //发布消息
```




