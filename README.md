# Front-end-note
## js知识点

- [js中的方法](#方法)
- [js中的变量](#变量)
- [闭包](#闭包)


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
  
  



