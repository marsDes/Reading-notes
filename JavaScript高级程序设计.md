# JavaScript高级程序设计 读书笔记 
## 1-2章略

## 3.1  语法
### 3.1.1  区分大小写
   test != Test
### 3.1.2  标识符
  指变量、函数、属性的名字或者函数的参数
  `var message = 'hello world',
       name    = 'mars',
       age     = 30'
  `
## 3.4 数据类型
*   基本数据类型：undefined、null、Boolean、number和string
*   复杂数据类型：object
    tips:如果定义的变量准备在将来用于保存对象，最好将改变量初始化为null
    
<pre>
  let person = null;
  if(person != null){
   //do someting
  }
</pre>
### 3.4.1 typeof 操作符
   返回值：1、undefined(未定义)；2、boolean(布尔值)；3、string(字符串)；4、number(数值)；5、object(对象或者null)；6、funtion(函数)

## 3.5 操作符
### 3.5.1 一元操作符
*   递增(++)和递减(--)操作符
   
<pre>
let age = 29;
    ++age;
let age = 29;
    age = age + 1;
let age = 29;
let otherAge = --age + 2;
console.log(age)   //28
console.log(otherAge) //30
</pre>
tips:前置执行操作符

*  加(+)和减(-)操作符
<pre>
 let a = 1, b = '1.1', c = 'z', d = false;
     a = +a;    // 1
     b = +b;    //1.1
     c = +c;    //NaN
     a = -a;    // -1
     b = -b;    // -1.1
     c = -c;    // NaN
     d = ±d;    //加减操作均返回 0
</pre>

### 3.5.2 位操作符
*   按位非        ~
*   按位与        &
*   按位或        |
*   按位异或      ^
*   左移         <<
*   有符号右移    >>
*   无符号右移    >>>
   
 ### 3.5.3 布尔操作符
*  逻辑非 !
   <pre>
   !false       //true
   !'false'     //false
   !0           //true
   !NaN         //true
   !''          //true
   !123         //false
   
   !!'false'   //true  双逻辑非操作符实际会模拟Boolean() 转型函数的行为，第一个逻辑非无论什么操作数均返回一个布尔值
   </pre>
*  逻辑与 &&
*  逻辑或 ||
   tips:可利用逻辑或避免变量null 或 undefined 例如：
   `let dataList = foodList || '' `
    
 ### 3.5.4 乘性操作符
