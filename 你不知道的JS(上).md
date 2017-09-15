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

### 2.2 this全面解析
