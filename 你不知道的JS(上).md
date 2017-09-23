# 1 作用域和闭包
待更...
# 2 this和对象原型
## 2.1 关于this
### 2.1.1 为什么使用this
```js
  function retName(){
    return this.name;
  }
  function sayHello(){
    console.log(`Hello,I'm ${retName.call(this)}`);
  }
  var me = {name:"Mars"}
  retName.call(me)                // Mars   this => me
  sayHello.call(me)               // Hello,I'm Mars  this => me
```
this，提供一种优雅的方式来隐式‘传递’一个对象的引用，可以是API设计更优雅简洁，并且容易复用。<br>
如不使用this就需要显示引入一个上下文对象，当使用的模式越来越复杂的时候，显示传递上下文对象会使代码越来越混乱。

### 2.1.2  误解
#### 2.1.2.1 指向自身
```js
  function foo(num){
    console.log(`foo: ${num}`);
    this.count++;              //在第一处for循环中 此处 创建了一个全局变量，因为this 指向 window   => var count; 
    console.log(this)          //在第一处for循环中 foo() 被 windows 调用
  }
  foo.count = 0;
  var i;
  for(i=0; i<5; i++){
    foo(i)                     //谁调用，指向谁 
  }
  console.log(foo.count)       // 0？  为什么 
  console.log(window.count)    // NaN  为什么  var count; undefined++ = NaN
  for(i=0; i<5; i++){
    //foo.call(foo,i)
    foo.apply(foo,[i])         //用 call apply 改变 this 指向
  }
  console.log(`foo.count now is ${foo.count}`)
```
### 2.1.3 小结
 <p style="color:#f54;">this 实际上是在函数被调用时发生绑定，它的指向完全取决于函数在哪里被调用</p>

## 2.2 this全面解析
### 2.2.1 调用位置
理解this的指向，首先要理解调用位置，即函数再代码中被调用的位置。分析调用栈（为了达到当前执行位置所调用的所有函数）
```js
  function funOne(){
    //当前调用栈是：funOne
    //因此当前调用的位置是全局作用域
    console.log('funOne')
    funTwo()
  }
  function funTwo(){
    //当前调用栈是：funOne -> funTwo
    //因此当前调用的位置是funOne中
    console.log('funTwo')
    funThree()
  }
  function funThree(){
    //当前调用栈是：funOne -> funTwo -> funThree
    //因此当前调用的位置是funTwo中
    console.log('funThree')
  }
  funOne()
```
### 2.2.2 绑定规则

#### 2.2.2.1 默认绑定
函数最常用的调用类型：独立函数调用，当无法应用其他规则的时候此规则作为默认规则
```js
  function funName(){
    console.log(this.a)
  }
  var a = 0;                  // 当 a 用 let 声明时打印 undefined
  funName()                   // 0; "use strict" 严格模式下 默认绑定不能绑定到全局对象
```

#### 2.2.2.2 隐式绑定
调用的位置是否有上下文对象，或是说是被某个对象拥有或者包含。当函数的引用有上下问对象时，隐式绑定规则会把this绑定到这个上下文对象。
隐式丢失，回调函数会导致丢失this绑定甚至调用回调函数的函数有可能会修改this。
```js
  function funName(){
    console.log(this.a)
  }
  var obj = {
    a:1,
    funName:funName           // 函数最后被obj调用，函数中的this 指向该对象即obj
  }
  var objX = {
    a:2,
    obj:obj
  }
  var a = 3;
  var funX = obj.funName;    // 函数别名！funX是obj.funName的一个引用，但实际上他引用的是 funName函数本身。
  obj.funName();             // 1; 
  objX.obj.funName();        // 对象属性引用链中只有上一层或者说最后一层再调用位置中起作用。
  funX()                     // 3 此处funX() 是一个不带任何修饰符的函数调用，因此应用了默认绑定
```

#### 2.2.2.3 显式绑定
利用call()，apply()方法强制绑定this,两者唯一区别主要体现在参数上<br>
如 [#### 2.1.2.1 所示](#2121-指向自身)
丢失绑定的解决方式
*   硬绑定 ES5内置方法 Funtion.prototype.bind,用法如下
```js
  function funName(arg){
    console.log(this.a,arg)
    return this.a + arg;
  }
  var obj = {a:2};
  var funBind = funName(20).bind(obj)   // 2,20
      console.log(funBind)              // 22
```
*   API调用的“上下文” context
```js
  function foo(el){
    console.log(el,this.id)
  }
  var obj = {id:"key"}
  [1,2,3].forEach(foo,obj)         //array.forEach(callback, this)
```

#### 2.2.2.4 new绑定
使用 new 来调用函数会执行如下操作
*   创建或者说构造一个全新的对象
*   这个新的对象会被执行[[Prototype]]连接
*   这个新的对象会绑定到函数调用的this
*   如果函数没有返回其他对象，那么new表达式在的函数调用会自动返回这个新的对象
```js
  function foo(a){
    this.a = a
  }
  var bar = new foo(1)
  console.log(bar)            // foo {a:1} 自动返回该新对象
  console.log(bar.a)          // 1
```
### 2.2.3 优先级别
_判断this_的顺序
1.   函数是否用了new调用，如果是的话 this 绑定的是新创建的对象 `var bar = new foo()`
2.   函数是否通过call，apply或者bind调用，是的话this绑定的是指定的对象 `var bar = foo.call(obj)`
3.   函数是否在某个上下文对象中调用(隐式绑定)，是的话this绑定的是那个上下文对象 `var bar = obj1.foo()`
4.   如果都不是的话，使用的是默认绑定，this绑定到全局对象，严格模式绑定到undefined `var bar = foo()`
### 2.2.4 绑定例外
#### 2.2.4.1 被忽略的this
如果把 _null_ 或者 _undefined_ 作为 call，apply或者bind指定的this，会被忽略，实际应用的是默认绑定规则
_更安全的this_
```js
  function foo(a,b){
    console.log(`a:${a},b:${b}`)
  }
  var x = Object.create(null)   // 比{}还空的对象 没有prototype  称为DMZ 空的非委托对象
  foo.apply(x,[2,3])
  //使用bind(..)进行柯里化
  var bar = foo.bind(x,3)      // 预先设定一些参数
  bar(3)                       // {a:3,b:3}
```
#### 2.2.4.2 间接引用
在创建一个函数的“间接引用”这种情况下，调用这个函数会应用默认绑定规则
```js
  function foo(){
    console.log(this.a)
  }
  var a = 2;
  var o = {a:3, foo:foo}
  var p = {a:4}
  o.foo()                        // 3
  (p.foo = o.foo)()              // 2 赋值表达式 返回目标函数的引用 foo(); 而不是p.foo() 或者 o.foo()
```
#### 2.2.4.3 软绑定
`fun.softBind(obj)  //P99`
### 2.2.5 this词法
箭头函数
```js
  function foo(){
    return (a) =>{
      //this 继承自 foo
      console.log(this.a)
    }
  }
  var obj1 = {a:1}
  var obj2 = {a:2}
  var bar = foo.call(obj1);
      bar.call(obj2)         // 1  箭头函数的this 无法改变 new 也不行 已绑定 obj1
```
### 2.2.6 小结
## 2.3 对象
## 2.4 混合对象“类”
### 2.4.1 类理论 
#### 2.4.1.1 "类"设计模式
#### 2.4.1.2 JavaScript中的类
### 2.4.2 类的机制
#### 2.4.2.1 建造
一个类就是一张设计图。为了获得真正可以交互的对象，我们必须按照类来建造（实例化）一个东西，这个东西通常被称为实例，
有需要的话你可以直接在实例上调用方法并访问其所有公共数据数据。
#### 2.4.2.2 构造函数
```js
class CoolGuy {
  specialSkill = nothing;
  CoolGuy(skill) {
    specialSkill = skill;
  }
  ShowOff(){
    outPut("here‘s my skill", specialSkill)
  }
}
Mars = new CoolGuy("Fly");
Mars.ShowOff()  // here's my skill Fly
```
CoolGuy类有一个构造函数CoolGuy()，这行new CoolGuy()，实际上调用的就是它，构造函数会返回一个对象（也就是类的一个实例）
类构造函数属于类，而且通常和类同名。
#### 2.4.3 类的继承
```js
// 定义一个 交通工具 类
class Vehicle{
  // 初始化 发动机个数
  engines = 1;
  // 定义点火操作
  ignition(){
    output("Turning on my engine")
  }
  // 定义驾驶方法
  drive(){
    ignition();
    output("Steering and moving forward")
  }
}
// 定义继承自Vehicle的交通工具car
class Car inherits Vehicle{
  // car 私有属性 轮子个数
  wheels = 4
  // car驾驶方法
  drive(){
    //继承父类 drive 方法
    inherited:drive();
    output("Rolling on all",wheels,"wheels")
  }
}
//定义继承自 Vehicle的交通工具 SpeedBoat
class SpeedBoat inherits Vehicle{
  // 重置 SpeedBoat 发动机个数
  engines = 2;
  // 重写 SpeedBoat 点火操作
  ignition(){
    output("Turning on my",engines,"engines.")
  }
  // 定义 SpeedBoat 驾驶方法
  polit(){
    inherited:drive(); //调用自身ignition方法
    output("Speeding through the water with ease!")
  }
}
```
