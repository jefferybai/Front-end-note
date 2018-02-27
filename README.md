# Front-end-note
## js知识点

- [js中的方法](#js-function)

## js-function
javascript的方法可以分为三类：

a 类方法

b 对象方法

c 原型方法

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

