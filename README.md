# Front-end-note
## js知识点

- [js中的方法](#方法)
- [js中的变量](#变量)


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

