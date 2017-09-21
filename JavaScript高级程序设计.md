# JavaScript高级程序设计 读书笔记 
## 1-2章 （略）

# 3 基本概念
## 3.1  语法
### 3.1.1  区分大小写
   test != Test
### 3.1.2  标识符
指变量、函数、属性的名字或者函数的参数
```js
  var message = 'hello world',
       name    = 'mars',
       age     = 30'
```
## 3.4 数据类型
*   基本数据类型：undefined、null、Boolean、number和string
*   复杂数据类型：object
    tips:如果定义的变量准备在将来用于保存对象，最好将改变量初始化为null
    
```js
  let person = null;
  if(person != null){
   //do someting
  }
```
### 3.4.1 typeof 操作符
   返回值：1、undefined(未定义)；2、boolean(布尔值)；3、string(字符串)；4、number(数值)；5、object(对象或者null)；6、funtion(函数)

## 3.5 操作符
### 3.5.1 一元操作符
*   递增(++)和递减(--)操作符
   
```js
let age = 29;
    ++age;
let age = 29;
    age = age + 1;
let age = 29;
let otherAge = --age + 2;
console.log(age)   //28
console.log(otherAge) //30
```
tips:前置执行操作符

*  加(+)和减(-)操作符
```js
 let a = 1, b = '1.1', c = 'z', d = false;
     a = +a;    // 1
     b = +b;    //1.1
     c = +c;    //NaN
     a = -a;    // -1
     b = -b;    // -1.1
     c = -c;    // NaN
     d = ±d;    //加减操作均返回 0
```

### 3.5.2 位操作符
*   按位非        ~
*   按位与        &
*   按位或        |
*   按位异或      ^
*   左移         <<
*   有符号右移    >>
*   无符号右移    >>>
   
 ### 3.5.3 布尔操作符
*   逻辑非 !
```js
   !false       //true
   !'false'     //false
   !0           //true
   !NaN         //true
   !''          //true
   !123         //false
   !!'false'   //true  双逻辑非操作符实际会模拟Boolean() 转型函数的行为，第一个逻辑非无论什么操作数均返回一个布尔值
```
*  逻辑与 &&
*  逻辑或 ||
   tips:可利用逻辑或避免变量null 或 undefined 例如：
   `let dataList = foodList || '' `
    
### 3.5.4  乘性操作符 （略）
### 3.5.6  加性操作符 （略）
### 3.5.7  关系操作符 （略）
### 3.5.8  条件操作符 
`let max = (num1 > num2) ? num1 : num2`
### 3.5.9  赋值操作符 （略）
### 3.5.10 逗号操作符 （略）

## 3.6 语句
### 3.6.1  if语句
```js
if(someVale){
 //your code1
}else{
 //your code2
}
```
### 3.6.2  do-while语句
 do-while 语句是一种后测试循环语句，即只有循环体中的代码执行之后，才会测试出口条件
```js
let start = 0;
do{
 start += 2;
}while(start > 10){  //只要 start 小于 10 就执行do 语句块
 console.log(start)
}
```
### 3.6.3  while语句
 while 语句属于前测试循环语句，即在循环体内的代码被执行前，就会对出口条件求值
```js
let start = 0;
while(start < 10){
 start += 2;
 console.log(start)
}
```
### 3.6.4  for语句 (略)
### 3.6.5  for-in语句
for-in语句是一种精准的迭代语句，可以用来美剧对象的属性
```js
for(let key in window){
 console.log(key)
}
```
### 3.6.6  label语句(略)
### 3.6.7  break和continue语句
break和continue语句用于再循环中精确地控制代码的执行。其中，break语句会立即退出循环，强制执行循环后的语句，而continue语句虽然也是立即退出循环，但退出循环后会从循环顶部继续执行。
```js
let num = 0;
for(let i=1; i<10; i++){
 if(i%5 == 0){
  break;         //中断循环  num++ 不执执行
 }
 num++
}
console.log(num)    //4
____________________________________________________________________________________________________
let num = 0;
for(let i=1; i<10; i++){
 if(i%5 == 0){
  continue;     // 跳出循环， num++ 不执行 ， 循环继续
 }
 num++
}
console.log(num)   //8
```
### 3.6.8  with语句（略）
### 3.6.9  switch语句
```js
switch(i){
  case 10:                      // 等价 if i === 10; 全等
    console.log('i is 10');
    break;                      // case 语句后面加break 中断执行，如省略即继续执行下面语句 合并多种情形
  case 20:
    console.log('i is 20');
    break;
  default:                     // 不匹配前面所有case语句则执行default 后面的语句
    console.log('other')
}
// case 语句也可以是变量或者是表达式
let num = 25;
switch(true){
  case num < 0:                // num < 0 === true 
    console.log('num is less than 0');
    break;
  case num >= 0 && num <= 20:
    console.log('num is between 0 and 20');
    break;
  default:
    console.log('num is more than 20')
}
```
## 3.7  函数
```js
function funName(arg0,arg1...){
  //your code 
}
funName(a,b,c...)
function returnSum(agr0,arg1){
  return arg0 + agr1
  console.log('never run')  //永远不会执行
}
let sum = returnSum(10,11);
```
推荐做法：要么让函数始终返回一个值要么永远都没返回值

### 3.7.1   理解参数
  在函数体内可以通过arguments(类数组非Array实例)对象访问参数数组，从而获取传递给函数的每一个函数，arguments[0],arguments[1]...，获取参数总数 arguments.length
重写 returnSum 函数
```js
function returnSum(){
  return arguments[0] + arguments[1];
}
```
由上说明:命名函数参数名只提供便利，但不是必需的。arguments行为也有一些比较有意思的
```js
function funA(x,y){
  arguments[1] = 10;
  console.log(x+y)
}
funA(10,100)    //20
funA(10)        // NaN  <== 10 + undefined   严格模式报错 代码(arguments[1] = 10;)不执行
```
arguments的长度由传入的参数个数决定，无法在函数内改变，未传递值得参数将自动赋值为undefined。同时也说明arguments非Array实例。
```js
let arrA = [];
    arrA[1] = 10;
    arrA = [undefined,10];
funA(10){
    //x = 10;
    arguments[1] = 10;
    //y = undefined;
}
```
### 3.7.2   没有重载
  可以通过对arguments的判断，执行不同代码，模仿重载

# 4 变量、作用域和内存问题
## 4.1 基本类型和引用类型的值
### 4.1.1 动态的属性
```js
let person = new Object();
    person.age = 30;
console.log(person.age)                 // 30
let name = 'mars';
    name.age = 30;
    console.log(name.age)               //undefined
```
### 4.1.2 复制变量值
```js
let num1 = 5,
    num2 = num1;
    num1 = 10;
    console.log(num2)                   // 5 num1互不影响
let personA = new Object(),
    personB = personA;
    personA.name = 'mars';
    console.log(personB.name)           //mars   
```
personA,personB 指向同一个对象，new Object()保存在堆内存中， personA,personB通过访问同一个指针实现对其引用
### 4.1.3 传递参数
```js
function resFun(num){
  return num + 10
}
let num = 20,
    newNum = resFun(num);   // num = 20;  newNum =30;

function setName(person){
  person.name = 'mars'
}
let person = new Object();
setName(person)             //person.name = 'mars'

function setName1(person){
  person.name = 'athean'
  person = new Object();
  person.name = 'mars'
}
let person1 = new Object();
    setName1(person1)      //person1.name = 'athean'
```
### 4.1.4 检测类型
*   typeof
```js
let a = 'aaaa',
    b = true,
    c = 22,
    d,
    e = null,
    f = {};
    typeof a;     // 'string'
    typeof b;     // 'boolean'
    typeof c;     // 'number'
    typeof d;     // 'undefined'
    typeof e;     // 'object'
    typeof f;     // 'object'
```
*   instanceof
```js
let person = {'name':'mars','age':30},
    colors = ['red','green','blue'],
    regExp = /[a-z]/;
    person instanceof Object;       //  变量 person 是 Object 吗 true
    colors instanceof Array;        //  true
    regExp instanceof RegExp;       //  true
```
 tips: instanceof 检测基本类型的值 始终返回 fasle 如: `a instanceof String` 

## 4.4 小结
  基本类型值和引用类型值有以下特点
*   基本值类型在内存中占据固定大小的空间，因此保存在栈内存中
*   从一个变量向另一个变量赋值基本类型的值，会创建这个值得副本
*   引用类型的值是对象，保存在堆内存中
*   包含引用类型值得变量实际上包含的并不是对象本身，而是一个指向该对象的指针
*   从一个变量向另一个变量赋值引用类型的值，实际复制的是指向对象的指针，因此2个变量最终指向同一个对象
*   typefo操作符可以确定值是哪种基本类型  instanceof操作符可以判断值是哪种引用类型

  作用域总结
*   作用域有全局作用域和函数作用域之分
*   每次进入一个新的执行环境，都会创建一个用于搜索变量和函数的作用域链
*   函数的局部作用域不仅可以访问函数作用域中的变量，也能放访问其父环境，甚至全局全局
*   全局环境只能访问在全局环境中定义的变量，而不能直接访问局部环境中的任何数据
*   变量的作用域有助于确定合适释放内存空间

  垃圾机制总结（略）

# 5 引用类型

## 5.1 Object 类型
  创建Object实例的2种方式
*   new操作符加Object构造函数
```js
let person =  new Object();  //person = {};
    person.name = 'mars';
    person.age = 18;
```
*   对象字面量
```js
let person = {
  name:'mars',
  age:18
}
```
  对象属性访问 点标识符及方括号，通常除了必须使用变量来访问属性，否则建议点标识法
```js
console.log(person.name)                  // mars
console.log(person["name"])               // mars
let propertyeName = 'name';
    console.log(person[propertyeName])    // mars
```

## 5.2 Array 类型
  创建Array实例的2种方式
*   new操作符加Array构造函数
```js
let colors = new Array();           // colors = Array()  可省略 new 操作符
let date = new Array(7);
let foods = new Array('seafood','rice','bread');
```
*   数组字面量
```js
let colors = [];
let foods = ['seafood','rice','bread'];
```
*   数组元素访问
```js
console.log(foods[0])      // seafood
foods[1] = 'meat'          // 修改第二项
foods[4] = 'vegetables'    // 新增第四项
foods.length               // 4
foods.length = 2           // 动态修改数组长度
console.log(foods[2])      // undefined
```

# 5.2.1 检测数组
```js
  let arr = [1,2,3,4,5];
  arr instanceof Array  // true
  Array.isArray(arr)    // true es6
```
# 5.2.2 相关方法
[数组相关方法](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)
```js
  let arr = [1,2,3,4,5,6];
  arr.push(7);                      //  返回数组长度  7
  arr.sort((a,b)=> a>b ? -1 : 1);   //[7, 6, 5, 4, 3, 2, 1]
  arr.splice(1,2)                   //返回被删除项 [6,5] 参数说明 起始下标 1，删除长度 2.
  console.log(arr)                  //[7, 4, 3, 2, 1]
  arr.splice(1,0,"6","5")           //返回被删除项 [],参数说明 起始下标1；删除长度 0 返回[]; 插入的项 "6","5";
  console.log(arr)                  //[7, "6", "5", 4, 3, 2, 1]
  arr.splice(1,2,6,5)               //返回被删除项 ["6","5"],参数说明 起始下标1；删除长度 2 ; 插入的项 6,5;
  console.log(arr)                  //[7, "6", "5", 4, 3, 2, 1]
```
# 5.2.8 迭代&&缩小方法
*   every()  : 对数组每一项运行给定函数,如该数组每一项都返回 true 则返回 true 类似 &&
*   some()   : 对数组每一项运行给定函数,如该数组中有一项返回 true 则返回 true 类似 ||
*   filter() : 对数组每一项运行给定函数,返回该函数返回 true 的项组成的数组 筛选
*   forEach(): 对数组每一项运行给定函数,无返回值  遍历
*   map()    : 对数组每一项运行给定函数,返回每次函数运行结果组成的数组<br>
    以上迭代方法调用给定函数的入参 item,idx,arr 
---
*   reduce()      : 从数组第一项开始逐项遍历到最后一项。
*   reduceRight() : 从数组最后一项开始向前遍历到第一项。<br>
    以上缩小方法2个入参: 1、每一项调用的函数; 2、可选。作为缩小基础的初始值
    传给调用函数的入参 pre cur idx arr,前一个值 当前值 项的索引 数组对象
```js
  let arr = [1,2,3,4,5,6],brr = [];
  let everyResult = arr.every((item,idx,arr) => item > 3)           // false
  let someResult = arr.some((item,idx,arr) => item > 3)             // true
  let filterResult = arr.filter((item,idx,arr) => item > 3)         //[4,5,6]
  let mapResult = arr.map((item,idx,arr) => item * 3)               //[3,6,9,12,15,18]
  arr.forEach(function(item,idx){
    brr.push(item * 2)
  });
  console.log(brr)                                                  //[2,4,6,8,10,12]
  let reduceResult = arr.reduce((pre,cur,idx,arr) => pre + cur)            // 21
      reduceResult = arr.reduce((pre,cur,idx,arr) => pre + cur, 9)         // 30
      reduceResult = arr.reduce((pre,cur,idx,arr) => pre + cur,'a')        // "a123456"
  let redRigResult = arr.reduceRight((pre,cur,idx,arr) => pre + cur, 9)    // 30
      redRigResult = arr.reduceRight((pre,cur,idx,arr) => pre + cur,'a')   // "a654321"
```

# 5.3 Date 类型 （略）
# 5.4 RegExp 类型 （略）
# 5.5 Function 类型
函数有如下定义方式：1、函数声明；2、函数表达式；3、Function 构造函数（不推荐）
```js
  function funName(a,b){
    return a + b
  }
  var sum = function(a,b){
    return a + b
  }
  var sum = new Function("a","b","return a+b")
```
## 5.5.1 没有重载
将函数名想象为指针
```js
  function funName(a,b){
    return a + b
  }
  function funName(a,b){
    return a - b
  }
  var res = funName(5,3);     // 2
  var a = 5;
      a = 6;
  console.log(a)              // 6 创建第二个函数的时候 实际覆盖了引用第一个函数变量 funName
```
## 5.5.2 函数声明与函数表达式  区别
是否提升
```js
  console.log(funName(5,5))   // 10
  function funName(a,b){
    return a + b
  }
  console.log(sum(5,5))       // 报错
  var sum = function(a,b){
    return a + b
  }
  var sum = new Function("a","b","return a+b")
```
## 5.5.3 做为参数的函数    (略)
## 5.5.4 函数内部属性
*   arguments   是类数组对象，用于保存函数参数，该对象有一个callee的属性，该属性是一个指针，指向拥有这个arguments对象的函数
*   this        函数的名字仅仅是一个包含指针的变量；同一个函数，被不同对象调用，该函数的指向仍然是同一个函数
*   caller(es6) 该属性保存着调用当前函数的函数的引用，如果在全局作用于调用该属性，它的值为null
```js
  function outer(){
    console.log(outer.caller)          // null 
    console.log(this.caller)           // undefined
    console.log(this == window)        // true
    inner()
  }
  function inner(){
    console.log(inner.caller == outer) // true
    console.log(this.caller)           // undefined
    console.log(this == window)        // true
  }
  outer()                             // 
```

## 5.5.5 函数属性和方法
属性
*   length 表示函数希望接收的命名参数的个数 `funName.length // 2`
*   prototype 第六章详细介绍 <br>
call(),apply(),bind()的作用及用法

## 5.6 基本包装类型   (略)
介绍 Boolean类型、Number类型、String类型相关方法 如，substr,replace...
## 5.7 单体内置对象
### 5.7.1 全局对象Gobal
*   window对象 web环境中的全局对象

### 5.7.2 Math对象
*   min()和max()方法，可以接收任意数量的参数 `Math.min(1,2,-3,5,0,4)  // -3`
*   舍入方法 ceil(),floor(),round()
```js
  var num = 2.34634
  Math.ceil(num)         // 3 向上取整
  Math.floor(num)        // 2 向下取整
  Math.round(num)        // 2 四舍五入
```
*   随机函数random(),返回0-1之间的随机数
```js
  Math.random()
  Math.floor(Math.random()*10 + 1)   // 1-10
  Math.floor(Math.random()*9 + 2)    // 2-10  2到10 有9个数
```

## 5.8 小结   (略)

# 6 面向对象的程序设计
面向对象编程（OOP）都有累的概念，而js没有，是通过闭包的封装来模拟类的实现。

## 6.1 理解对象
创建方式 new 构造函数及对象字面量

### 6.1.1 属性类型
*   _数据属性_ 包含一个数据值的位置，其有四个描述其行为的特性。
  1. [[Configurable]]:表示能否通过delete删除属性从而重新定义属性  默认值为 true
  2. [[Enumerable]]:表示能否通过for-in循环返回属性                默认值为 true
  3. [[Writable]]:表示能否修改属性的值                            默认值为 true
  4. [[Vaule]]:包含这个属性的数据值                               默认值为 undefined
```js
//修改属性默认特性 方法  Object.defineProperty(obj,key,descriportObj)
//descriportObj 的属性名必须是 configurable|enmuberable|writeable|value
  var obj = {
    name:"Mars",
    age:30
  }
  Object.defineProperty(obj,"name",{
    value:"Kobe",
    configurable:false,
    writable:false
    });
  console.log(obj)         // {name:"Kobe",age:30}
  obj.name = "Apple";
  delete obj.name
  delete obj.age
  console.log(obj)         // {name:"Kobe"}
```
*   _访问器属性_ 不包含数据值，他们包含一对getter和setterh函数，其有如下四个特性
  1. [[Configurable]]:表示能否通过delete删除属性从而重新定义属性  默认值为 true
  2. [[Enumerable]]:表示能否通过for-in循环返回属性                默认值为 true
  3. [[Get]]:读取属性时调用的函数                                 默认值为 undefined
  4. [[Set]]:写入属性时调用的函数                                 默认值为 undefined

### 6.1.2 定义多个属性   (略)
### 6.1.4 读取属性的特性   (略)
## 6.2 创建对象
### 6.2.1 工厂模式
工厂模式能被无数次调用，创建多个相似的对象，但不能解决对象识别问题（即怎么知道一个对象的类型）
```js
  function getPerson(name,age,job){
    var o = new Object();
    o.name = name;
    o.age = age;
    o.job = job;
    o.sayName = function(){
      console.log(`My name is ${this.name}`)
    }
    return o;
  }
```
### 6.2.2 构造函数模式
首字母大写，以区分其他函数。
```js
  function Person(name,age,job){
    this.name = name;
    this.age = age;
    this.job = job;
    this.sayName = function(){
      console.log(`My name is ${this.name}`)
    }
  }
  var per1 = new Person("mars",30,"painter")
  var per2 = new Person("Kobe",30,"player")
  console.log(per1.constructor == per2.constructor)    // true per1,per2的构造函数都指向Person
  console.log(per1 instanceof Object)     // true
  console.log(per1 instanceof Person)     // true per1 是Person的一个实例
```
*   将构造函数当做函数
```js
  var per3 = new Person("Tom",19,"student")
  per3.sayName()                         // Tom
  Person("Window",99,"Manager")
  window.sayName()                       // Window 不是用new操作符时，被全局调用，this => window
```
*   构造函数的问题

### 6.2.3 原型模式
我们每创建一个函数都有一个protopyte(原型)属性，这个属性是一个指针，指向一个对象，这个对象用途是包含可以由特定类型的所有实例共享的属性和方法
```js
  function Person(){
  }
  Person.prototype.name = "Mars";
  Person.prototype.age = 30;
  Person.prototype.job = "painter";
  Person.prototype.sayName = function(){
    console.log(`My name is ${this.name}`)
  }
  var per1 = new Person();
  var per2 = new Person();
```
*   理解原型对象 <br>
    每创建一个函数都会，就为该函数创建prototype属性，这个属性指向函数的原型对象，该对象最初只含constructor(构造函数)属性。
    如果为构造函数创建的实例添加原型链上同名的属性，该属性会屏蔽原型中的那个属性,而不是修改原型中那个属性。
    obj.hasOwnProperty("keyName")可判断属性 keyName 是否存在实例中，还是原型中
```js
  var per3 = new Person();
  per3.name = "Applen";
  console.log(per3.name)   // Applen
  console.log(per1.name)   // Mars
  delete per3.name         // 删除实例属性
  console.log(per3.name)   // Mars
```
*   原型与in操作符
```js
  per1.hasOwnProperty("name");   //flase
  "name" in per1                 //true
  Object.keys(Person.prototype)  // ["name","age", "job","sayName"]
  var per4 = new Person();
  per4.name = "Jim";
  per4.age = 21;
  Object.keys(per4)              // ["name","age"]
```

*   更简单的原型语法
```js
  function Person(){}
  //重写函数原型，创建新对象
  Person.prototype = {
    name:"mars",
    age:"30",
    job:"painter",
    sayName:function(){
      console.log(this.name)
    }
  }
  Person.prototype.constructor == Person     // false  
  Person.prototype.constructor == Object     // true
  // constructor 为新属性的 constructor属性 指向 Object 构造函数
```

*   原型的动态性
```js
  var per5 = new Person();       //创建实例
  Person.prototype.sayHi = function(){console.log("Hi")}
  per5.sayHi()                   // Hi
  //创建实例后为原型添加新属性，该实例仍能访问新增的属性
  function Foods(){}
  var bread = new Foods();
  Foods.prototype = {
    name:"bread",
    sayName:function(){
      console.log("i taste good")
    }
  }
  var cheese = new Foods()
  bread.sayName()              //Uncaught TypeError
  Foods.prototype.price = "$12"
  console.log(bread.price)    // undefined
  console.log(cheese.price)   // $12
```

## 6.3 继承
ECMAScript支持实现继承，而且实现继承主要依靠原型链实现
## 6.3.1 原型链
基本思想：利用原型让一个引用类型继承另一个引用类型的属性和方法。
构造函数、原型和实例的关系：每个构造函数都有一个原型对象（prototype）原型对象都包含一个指向构造函数的指针（constructor），
而实例都包含一个指向原型对象的指针（[[Prototype]]）
```js
function SuperType(){
  this.proterty = "superValue";
}
SuperType.prototype.getSuperValue = function(){
  return this.proterty;
}
function SubType(){
  this.subproterty = "subValue";
}
SubType.prototype = new SuperType();
SubType.prototype.getSubValue = function(){
  return this.subproterty
}
var intance = new SubType();
console.log(intance.getSubValue())              // superValue
console.log(intance.getSuperValue())            // subValue
console.log(intance.constructor == SuperType)   // true
```
*   _别忘记默认原型_ 所有引用类型都继承了Object，所有函数默认原型的实例都是Object,因此默认原型都包含一个内部指针，指向Object.prototype
*   _确定原型与实例的关系_ `intance instanceof Object || Object.prototype.isPrototypeOf(intance)`
*   _谨慎定义方法_
*   _原型链的问题_ 1、原型属性会被所有实例共享；2、不能向超类型的构造函数传递参数


