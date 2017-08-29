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
    
  `
  let person = null;
  if(person != null){
   //do someting
  }
  ` 
### 3.4.1 typeof 操作符
   返回值：1、undefined(未定义)；2、boolean(布尔值)；3、string(字符串)；4、number(数值)；5、object(对象或者null)；6、funtion(函数)

## 3.5 操作符
### 3.5.1 一元操作符
   * 递增(++)和递减(--)操作符
   
    `
    let age = 29;
        ++age;
    let age = 29;
        age = age + 1;
    `
tips:前置执行操作符
