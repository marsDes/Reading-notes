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
利用call()，apply()方法强制绑定this,2者唯一区别主要体现在参数上<br>

如 [#### 2.1.2.1 所示](#### 2.1.2.1 指向自身)<br>
如 [#### 2.1.2.1 所示](http://www.baidu.com)<br>
