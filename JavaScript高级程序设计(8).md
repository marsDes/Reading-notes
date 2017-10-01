# 8 BOM
## 8.1 window对象
window对象是BOM的核心，他表示一个浏览器的一个实例，是js访问浏览器窗口的一个接口也是ECMAScript规定的global对象
### 8.1.1 全局作用域
```js
var age = 30;
function sayAge(){
  console.log(this.age)
}
console.log(window.age == age)  // true
console.log(window.sayAge())    // 30
```
### 8.1.2 窗口关系及框架
如果页面中包含框架，则每个框架都拥有自己的window对象，并保存在frames集合中。
### 8.1.3 窗口位置
### 8.1.4 窗口大小
*   _innerWidth_ 及 _innerHeight_ 容器中页面视图区域的大小（减去边框宽度）
*   _outerWidth_ 及 _outerHeight_ 浏览器窗口本身的尺寸

### 8.1.5 导航和打开窗口
*   弹开窗口
```js"
wind"wopen("http://github.com","_blank","",true)    ,
// 第四个参数 新的页面是否取代当前浏览器历史记录中但钱加载页面的布尔值
window.close()  // 只能关闭由window.open打开的窗口
```
*   安全限制

### 8.1.6 间歇调用和超时调用
```js
//创建超时调用
var timeEr1 = setTimeout(function(){console.log("go on")},1000);
//创建间歇调用
var timeEr2 = setInterval(function(){console.log("go on")},1000);
// 记得离开窗口要清除！！！
clearTimeout(timeEr1)
clearInterval(timeEr2)
```
### 8.1.7 系统提示框
## 8.2 location对象
location对象既是window对象属性也是document对象的属性。
```js
window.location == document.location //true
{ 
  "ancestorOrigins":DOMStringList {length: 0},,
  "assign":ƒ (),
  "hash":"#ABC12",
  "host":"www.zhihu.com",
  "hostname":"www.zhihu.com",   //
  "href":"https://www.zhihu.com:80/question/20790576?name=mars#ABC12",  //当前加载页面完整的url
  "origin":"https://www.zhihu.com",
  "pathname":"/question/20790576",  // 路径或文件名
  "port":"80",   //url 指定的端口号 没有显示“”
  "protocol":"https:", // 传输协议
  "reload":ƒ reload(),
  "replace":ƒ (),
  "search":"?name=mars",  //返回url查询的字符串 以？开头
  "toString":ƒ toString(),
  "valueOf":ƒ valueOf(),
  "Symbol(Symbol.toPrimitive)":undefined,
  "__proto__":Location
}
```
## 8.2.1 查询字符串参数
`document.location.search`
## 8.2.2 位置操作
```js
// 以下三种方式效果相同
location.assign("http://www.baidu.com");
window.location = "http://www.baidu.com";
location.href = "http://www.baidu.com";
// 不写入浏览历史
location.replcae("http://www.baidu.com");
//重新加载当前页面
location.reload(true)      // 默认false 有可能从缓存中加载数据，true 从服务器重新记载
```
# 8.3 navigator对象
```js
{
  "appCodeName":"Mozilla",
  "appName":"Netscape",
  "appVersion":"5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/55.0.2883.87 Safari/537.36",
  "cookieEnabled":true,
  "credentials":CredentialsContainer,
  "doNotTrack":null,
  "geolocation":Geolocation,
  "hardwareConcurrency":8,
  "language":"zh-CN",
  "languages":Array[2],
  "maxTouchPoints":0,
  "mediaDevices":MediaDevices,
  "mimeTypes":MimeTypeArray,
  "onLine":true,
  "permissions":Permissions,
  "platform":"Win32",
  "plugins":PluginArray,
  "presentation":Presentation,
  "product":"Gecko",
  "productSub":"20030107",
  "serviceWorker":ServiceWorkerContainer,
  "storage":StorageManager,
  "userAgent":"Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/55.0.2883.87 Safari/537.36",
  "vendor":"Google Inc.",
  "vendorSub":"",
  "webkitPersistentStorage":DeprecatedStorageQuota,
  "webkitTemporaryStorage":DeprecatedStorageQuota
}
```
## 8.4 screen对象
## 8.5 history对象
## 8.6 小结
# 9 客户端检测   （略）
# 10 DOM
## 10.1 节点层次
### 10.1.2 Document类型
```js
document  //整个HTML页面
var body = document.body  //取得页面body节点的引用
//查找元素  主语复数s
var domId = document.getElementById("domId");         //获取id为domId的节点
var domtaget = document.getElementsByTagName("img");   //获取所有img标签节点
var domClass = document.getElementsByClassName("domClass"); //
```
### 10.1.3 Element类型
```js
//访问元素标签名
console.log(domId.tagName)   // DIV 大写
domId.id   // 获取domId的 Id（title,className...）
domId.id = "newDomId"   //修改domId的ID 值
```
*   HTML元素
```js
//设置属性相关方法
domId.getAttribute("title")   // 获取属性
domId.setAttribute("id","domId")   // 设置属性
domId.removeAttribut("class") // 删除属性
//创建元素
var cDiv = document.creatElement("div");
cDiv.id = "Mars";
//插入元素
document.body.appendChild(cDiv);
console.log(document.getElementById("cDiv"));   // <div id="Mars"></div>
```
### 10.1.4 Text类型
### 10.1.5 Comment类型

# 11 DOM扩展
### 11.1.1 querySelector()方法
querySelector()方法，接收一个css选择符，返回与该模式匹配的 _第一个_ 元素，没有则返回null
`querySelector('#divId');querySelector('body')`
### 11.1.2 querySelectorAll()方法
querySelectorAll()，接收一个css选择符，返回所有与该模式匹配的所有元素。
`querySelectorAll('div');querySelectorAll('.red')`
### 11.1.3 matchesSelector()方法
matchesSelector(),接收一个css选择符，如果调用的元素与改选择符匹配则返回true。
## 11.2 元素遍历
*   childElementCount; 返回子元素个数
*   firstElementChild; 指向第一个子元素
*   lastElementChild; 指向最后一个子元素
*   previousElementSibiling; 指向前一个同辈元素
*   nextElementSibiling; 指向后一个同辈元素
```js
var body = document.body;
var firstChild = body.firstElementChild;
var classArr = body.classList;  //["classA","classB",...]
```

## 11.3 HTML
### 11.3 与类相关的扩充
*   getElementsByClassName() ; 接收一个css 类选择符
*   classList属性;
*   classList.add();// 将给定的字符串值添加到列表中，有就不添加
*   classList.contains();//判断给点的字符串值是否存在列表中，有则返回true
*   classList.remove();//将给定的字符串值从列表中删除
*   classList.toggle();//如果列表中有给定字符串的值；则删除它，没有则添加上

### 11.3.2 焦点管理
document.activeElement 属性，这个属性始终会引用DOM中当前获取焦点的元素
```js
var btn = document.getElementById("btn");
btn.focus();
document.activeElement == btn    // true
```
### 11.3.3 HTMLdocument的变化
1.   readyState属性的值：loading(正在加载文档)；complete(已经加载完文档)
2.   兼容模式
3.   head属性 document.head

### 11.3.4 字符集属性
### 11.3.6 插入标记
1.   innerHTML; btn.innerHTML = 'MARS';优化 拼接html字符串再执行innerHTML操作
2.   outerHTML; btn.outerHTML = 'LIN'

### 11.3.7 scrollIntoViw()方法 
如果给这个方法传 _true_或者不传任何参数，那么窗口滚动之后会调用元素的顶部与视口尽可能平齐
如果给这个方法传 _false_那么窗口滚动之后会调用元素的底部与视口尽可能平齐

### 11.4.4 插入文本
1.   innerText; btn.innerText = 'MARS';优化 拼接html字符串再执行innerText操作
2.   outerText; btn.outerText = 'LIN'
3.   非html5规范属性

## 11.5 小结

# 12 DOM2和DOM3
## 12.2 样式
### 12.2.1 访问元素的样式
任何支撑style特性的HTML元素在js中都有一个对象的style属性
```js
var div = document.getElementById("divID");
// 获取元素样式
var divW = div.style.width;   
var divC = div.style.color;  //rgb(255,255,255)
// 设置元素样式
div.style.color = '#f65';
div.style.width = '100px';
div.style.backgroundColor = '#f73260';
```
