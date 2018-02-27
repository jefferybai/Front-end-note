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
  this.Introduce=function(){
    alert("My name is "+this.name);
  }
}
//类方法
People.Run=function(){
  alert("I can run");
}
//原型方法
People.prototype.IntroduceChinese=function(){
  alert("我的名字是"+this.name);
}
