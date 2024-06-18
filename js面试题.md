---
title: js面试题
date: 2023-07-17 22:04:25
description: js的一些面试题，比较考研基本功夫
tags: 
  - js
  - 暑假学习
categories:
cover: https://img.znxs.vip/znxs/GMI_%E7%8E%8B%E5%86%A0%E5%B0%91%E5%A5%B3.jpg
top_img: https://img.znxs.vip/znxs/GMI_%E7%8E%8B%E5%86%A0%E5%B0%91%E5%A5%B3.jpg
---

#### 1.JavaScript是一门什么样的语言，它有哪些特点？（没有标准答案）

参考答案：
JavaScript是一门强大的编程语言，是一种专为与网页交互而设计的脚本语言，是一种动态类型、弱类型、基于原型的语言。

JavaScript是客户端和服务器端脚本语言，可以插入到HTML页面中，并且是目前较热门的Web开发语言。同时，JavaScript也是面向对象编程语言。

特点：

1. 脚本语言。JavaScript是一种解释型的脚本语言,C、C++等语言先编译后执行,而JavaScript是在程序的运行过程中逐行进行解释。
2. 基于对象。JavaScript是一种基于对象的脚本语言,它不仅可以创建对象,也能使用现有的对象。
3. 简单。JavaScript语言中采用的是弱类型的变量类型,对使用的数据类型未做出严格的要求,是基于Java基本语句和控制的脚本语言,其设计简单紧凑。
4. 动态性。JavaScript是一种采用事件驱动的脚本语言,它不需要经过Web服务器就可以对用户的输入做出响应。在访问一个网页时,鼠标在网页中进行鼠标点击或上下移、窗口移动等操作JavaScript都可直接对这些事件给出相应的响应。
5. 跨平台性。JavaScript脚本语言不依赖于操作系统,仅需要浏览器的支持。因此一个JavaScript脚本在编写后可以带到任意机器上使用,前提上机器上的浏览器支 持JavaScript脚本语言,目前JavaScript已被大多数的浏览器所支持。

#### 2.JavaScript的数据类型都有什么？

- 基本数据类型：Number、String、Boolean、Null、Undefined
- 复杂数据类型：Object（Function、Array、Date、RegExp）

**如何判断某变量是否为数组数据类型？**

```js

if (typeof Array.isArray === "undefined"){
    Array.isArray = function(arg){
        return Object.prototype.toString.call(arg)==="[object Array]";
    }
}
```

#### 3.已知ID的Input输入框，希望获取这个输入框的输入值，怎么做？(不使用第三方框架)

```

document.getElementById(ID).value;
```

#### 4.希望获取到页面中所有的checkbox怎么做？(不使用第三方框架)

```js

var inputs = document.getElementsByTagName("input"),
    arr = [],
    len = inputs.length;
while (len--){
    if(inputs[len].type == "checkbox"){
        arr.push(inputs[i]);
    }
}
```

#### 5.设置一个已知ID的DIV的html内容为xxxx，字体颜色设置为黑色(不使用第三方框架)

```js

var oDiv = document.getElementById(ID);
oDiv.innerHTML = "xxxx";
oDiv.getElementById(ID).style.color = "black";
```

#### 6.当一个DOM节点被点击时候，我们希望能够执行一个函数，应该怎么做？

先获取到这个DOM节点，然后绑定onclick事件。比如`myDOM.onclick = fn`或者`myDOM.addEventListener("click",fn);`

或者直接在HTML中绑定`<div onclick = "fn()"></div>`

#### 7.什么是Ajax和JSON，它们的优缺点。

Ajax是异步JavaScript和XML，用于在Web页面中实现异步数据交互。

优点：

- 可以使得页面不重载全部内容的情况下加载局部内容，降低数据传输量•避免用户不断刷新或者跳转页面，提高用户体验

缺点：

- 对搜索引擎不友好
- 要实现ajax下的前后退功能成本较大
- 可能造成请求数的增加
- 跨域问题限制

JSON是一种轻量级的数据交换格式，ECMA的一个子集

优点：轻量级、易于人的阅读和编写，便于机器（JavaScript）解析，支持复合数据类型（数组、对象、字符串、数字）

#### 8.看下列代码输出为何？解释原因。

```js

var a;
alert(typeof a); //undefined
alert(b); //报错
```

变量a是被声明的，只是没有赋值，所以值为undefined。而b未被声明，所以不存在。

#### 9.看下列代码,输出什么？解释原因。

```js

var a = null;
alert(typeof a); //object
```

null是一个空指针对象，所以类型是object。

#### 10.看下列代码,输出什么？解释原因。

```js

var undefined;
undefined == null; //true
1 == true; //true
2 == true; //false
0 == false; //true
0 == ''; // true
NaN == NaN; //false
[] == false; //true
[] == ![]; //true
```

- undefined与null在if语句中会被自动转为false，相等运算符直接报告两者相等。（如果是全等的话结果为false）
- 数字和布尔值进行比较会把布尔值变为数字，true为1，false为0。
- 0为假即false，空值或者空格也为false。
- NaN和任何值都不相等。
- []被当作数组处理，空数组转换为0，所以等于false。
- ![]想做将数组转换为布尔值的运算，[]为一个数组对象，所以![]为false。

**看下面的代码，输出什么，foo的值为什么？**

```js

var foo = "11"+2-"1";
console.log(foo); //111
```

先做”11”+2运算，当一个为字符串一个为数字时，将数字转换为字符串，所以字符串拼接为”112”。当两个数据都是字符串时，+以外的运算会先把字符串转换为数字，即112-1=111,foo类型为Number。如果是+运算，则为”112”+“1”=“1121”，foo类型为String。

#### 11.看代码给答案。

```js

var a = new Object();
a.value = 1;
b = a;
b.value = 2;
alert(a.value); //2
```

a,b都指向同一个对象，所以b修改了value值，a的value值也变了。

#### 12.已知数组var stringArray = [“This”, “is”, “Baidu”, “Campus”]，Alert出”This is Baidu Campus”。

stringArray.join(“ “);

**已知有字符串foo=“get-element-by-id”,写一个function将其转化成驼峰表示法“getElementById”。**

```js

var fooArr = foo.split("-");
var newFoo = fooArr[0];
for(var i=1;i<fooArr.length;i++){
    newFoo += fooArr[i].charAt(0).toUpperCase()+fooArr[i].slice(1);
}
return newFoo;
```

#### 13.var numberArray = [3,6,2,4,1,5];

1.实现对该数组的倒排，输出[5,1,4,2,6,3]`numberArray.reverse();`2.实现对该数组的降序排列，输出[6,5,4,3,2,1]

```js

numberArray.sort(function(a,b){
       return b-a;
   });
```

#### 14.输出今天的日期，以YYYY-MM-DD的方式，比如今天是2014年9月26日，则输出2014-09-26

```js

var date = new Date();
var year = date.getFullYear();
var month = date.getMonth()+1;
var nowDate = date.getDate();
if(month<10){month = "0" + month;}
if(nowDate<10){nowDate = "0" + nowDate;}
var today = year + "-" + month + "-" + nowDate;
```

#### 15.将字符串“{KaTeX parse error: Expected ‘EOF’, got ‘}’ at position 3: id}̲<…name}”中的{KaTeX parse error: Expected ‘EOF’, got ‘}’ at position 3: id}̲替换成10，{name}替换成Tony。（使用正则表达式）

```js

var str = "<tr><td>{$id}</td><td>{$name}</td></tr>";
str.replace(/\{\$id\}/,"10").replace(/\{\$name\}/,"Tony");
```

#### 16.为了保证页面输出安全，我们经常需要对一些特殊的字符进行转义，请写一个函数escapeHtml，将<, >, &, \进行转义

```js


function escapeHTML(str){
    return str.replace(/[<>&"]/g,function(match){
        switch(match){
            case "<":
            return "\<";
            case ">":
            return "\>";
            case "&":
            return "\&";
            case "\"":
            return "\"";
        }
    });
}
```

#### 17.foo = foo||bar ，这行代码是什么意思？为什么要这样写？

如果foo不为假则使用原来的值，没有值则把bar的值付给foo。

#### 18.看下列代码，将会输出什么?(变量声明提升)

```js

var foo = 1;
function(){
    console.log(foo); //undefined
    var foo = 2;
    console.log(foo); //2
}
```

函数声明和变量声明会被隐式地提升到当前作用域的顶部，但是不会提升赋值部分。相当于：

```js

var foo = 1;
function(){
    var foo;
    console.log(foo); //undefined
    foo = 2;
    console.log(foo); //2
}
```

#### 19.用js实现随机选取10–100之间的10个数字，存入一个数组，并排序。

```js

var arr = [];
for(var i=0;i<10;i++){
    arr.push(Math.random()*0.9*100+10);
}
arr.sort(function(a,b){return a-b;});
```

#### 20.把两个数组合并，并删除第二个元素。

var arr1 = [1,2,3];
var arr2 = [“a”,“b”,“c”];
var newArr = arr1.contant(arr2);
newArr.splice(1,1);

#### 21.怎样添加、移除、移动、复制、创建和查找节点

```js

- appendChild() //添加
- reomveChild() //移除
- insertBefore() //移动
- cloneNode() //复制
- createElement();createTextNode();createDocumentFragment //复制
- getElementById();getElementsByTagName();getElementsByClassName();getElementsByName() //查找
```

#### 22.有这样一个URL：`http://item.taobao.com/item.htm?a=1&b=2&c=&d=xxx&e`，请写一段JS程序提取URL中的各个GET参数(参数名和参数个数不确定)，将其按key-value形式返回到一个json结构中，如{a:’1′, b:’2′, c:”, d:’xxx’, e:undefined}。

```js

var url = "http://item.taobao.com/item.htm?a=1&b=2&c=&d=xxx&e";
var gets = url.split("?")[1];
var getsArr = gets.split("&");
var obj = {};
for(var i=0;i<getsArr.length;i++){
    obj[getsArr[i].split("=")[0]] = getsArr[i].split("=")[1];
}
return obj;
```

#### 23.正则表达式构造函数var reg=new RegExp(“xxx”)与正则表达字面量var reg=//有什么不同？匹配邮箱的正则表达式？

当使用RegExp()构造函数的时候，不仅需要转义引号（即”表示”），并且还需要双反斜杠（即\表示一个\）。

```js

/^([A-Za-z0-9-_])+@([A-Za-z0-9]+.)+([a-z])+$/
```

#### 24.看下面代码，给出输出结果。

```js

for(var i=1;i<=3;i++){
  setTimeout(function(){
  console.log(i); //4 //4 //4
  },0);  
};
```

Javascript事件处理器在线程空闲之前不会运行。

**如何让上述代码输出1 2 3？**

```js

for(var i=1;i<=3;i++){
  setTimeout((function(i){
  console.log(i);
  })(i),0);  //立即执行函数
};
```

#### 25.写一个function，清除字符串前后的空格。（兼容所有浏览器）

```js

if(!String.prototype.trim){
    String.prototype.trim = function(){
        return this.replace(/^\s+/,"").replace(/\s+$/,"");
    }
}
```

#### 26.Javascript中callee和caller的作用？

**如果一对兔子每月生一对兔子；一对新生兔，从第三个月起就开始生兔子；假定每对兔子都是一雌一雄，试问一对兔子，第n个月能繁殖成多少对兔子？（使用callee完成）**

```js


var result = [];
function fn(n){
    if(n==1){
        return 1;
    }else if(n==2){
        return 1;
    }else{
        result[n] = arguments.callee(n-2)+arguments.callee(n-1);
        return result[n];
    }
}
```

#### 27. 解释什么是回调函数，并提供一个简单的例子。

回调函数是可以作为参数传递给另一个函数的函数，并在某些操作完成后执行。下面是一个简单的回调函数示例，这个函数在某些操作完成后打印消息到控制台。

```js

function modifyArray(arr, callback) {
 // 对 arr 做一些操作
 arr.push(100);
 // 执行传进来的 callback 函数
 callback();
}
var arr = [1, 2, 3, 4, 5];
modifyArray(arr, function() {
 console.log("array has been modified", arr);
});
```

#### 28. 如何检查一个数字是否为整数？

检查一个数字是小数还是整数，可以使用一种非常简单的方法，就是将它对 1 进行取模，看看是否有余数。

```js

function isInt(num) {
 return num % 1 === 0;
}
console.log(isInt(4)); // true
console.log(isInt(12.2)); // false
console.log(isInt(0.3)); // false
```

#### 29. 你能解释一下 ES5 和 ES6 之间的区别吗？

ECMAScript 5（ES5）：ECMAScript 的第 5 版，于 2009 年标准化。这个标准已在所有现代浏览器中完全实现。
ECMAScript 6（ES6）或 ECMAScript 2015（ES2015）：第 6 版 ECMAScript，于 2015 年标准化。这个标准已在大多数现代浏览器中部分实现。
以下是 ES5 和 ES6 之间的一些主要区别：

```js

// 箭头函数和字符串插值：
const greetings = (name) => {
 return `hello ${name}`;
}
const greetings = name => `hello ${name}`;
```

#### 30. Javascript 中的“闭包”是什么？举个例子？

闭包是在另一个函数（称为父函数）中定义的函数，并且可以访问在父函数作用域中声明和定义的变量。

闭包可以访问三个作用域中的变量：

- 在自己作用域中声明的变量；
- 在父函数中声明的变量；
- 在全局作用域中声明的变量。

```js


var globalVar = "abc";
// 自调用函数
(function outerFunction (outerArg) { // outerFunction 作用域开始
 // 在 outerFunction 函数作用域中声明的变量
 var outerFuncVar = 'x'; 
 // 闭包自调用函数
 (function innerFunction (innerArg) { // innerFunction 作用域开始
 // 在 innerFunction 函数作用域中声明的变量
 var innerFuncVar = "y";
 console.log( 
 "outerArg = " + outerArg + "
" +
 "outerFuncVar = " + outerFuncVar + "
" +
 "innerArg = " + innerArg + "
" +
 "innerFuncVar = " + innerFuncVar + "
" +
 "globalVar = " + globalVar);
 // innerFunction 作用域结束
 })(5); // 将 5 作为参数
// outerFunction 作用域结束
})(7); // 将 7 作为参数
```

innerFunction 是在 outerFunction 中定义的闭包，可以访问在 outerFunction 作用域内声明和定义的所有变量。除此之外，闭包还可以访问在全局命名空间中声明的变量。

上述代码的输出将是：

```js

outerArg = 7
outerFuncVar = x
innerArg = 5
innerFuncVar = y
globalVar = abc
```

#### 31. “this”关键字的原理是什么？请提供一些代码示例。

在 JavaScript 中，this 是指正在执行的函数的“所有者”，或者更确切地说，指将当前函数作为方法的对象。

```js


function foo() {
 console.log( this.bar );
}
var bar = "global";
var obj1 = {
 bar: "obj1",
 foo: foo
};
var obj2 = {
 bar: "obj2"
};
foo(); // "global"
obj1.foo(); // "obj1"
foo.call( obj2 ); // "obj2"
new foo(); // undefined
```

#### 32. 如何向 Array 对象添加自定义方法，让下面的代码可以运行？

JavaScript 不是基于类的，但它是基于原型的语言。这意味着每个对象都链接到另一个对象（也就是对象的原型），并继承原型对象的方法。你可以跟踪每个对象的原型链，直到到达没有原型的 null 对象。我们需要通过修改 Array 原型来向全局 Array 对象添加方法。

```js

Array.prototype.average = function() {
 // 计算 sum 的值
 var sum = this.reduce(function(prev, cur) { return prev + cur; });
 // 将 sum 除以元素个数并返回
 return sum / this.length;
}
var arr = [1, 2, 3, 4, 5];
var avg = arr.average();
console.log(avg); // => 3
```

#### 33. 请描述一下 Revealing Module Pattern 设计模式。

暴露模块模式（Revealing Module Pattern）是模块模式的一个变体，目的是维护封装性并暴露在对象中返回的某些变量和方法。如下所示：

```js


var Exposer = (function() {
 var privateVariable = 10;
 var privateMethod = function() {
 console.log('Inside a private method!');
 privateVariable++;
 }
 var methodToExpose = function() {
 console.log('This is a method I want to expose!');
 }
 var otherMethodIWantToExpose = function() {
 privateMethod();
 }
 return {
 first: methodToExpose,
 second: otherMethodIWantToExpose
 };
})();
Exposer.first(); // 输出: This is a method I want to expose!
Exposer.second(); // 输出: Inside a private method!
Exposer.methodToExpose; // undefined
```

它的一个明显的缺点是无法引用私有方法。

#### 34. 列举Java和JavaScript之间的区别？

Java是一门十分完整、成熟的编程语言。相比之下，JavaScript是一个可以被引入HTML页面的编程语言。这两种语言并不完全相互依赖，而是针对不同的意图而设计的。 Java是一种面向对象编程（OOPS）或结构化编程语言，类似的如C ++或C，而JavaScript是客户端脚本语言，它被称为非结构化编程。

#### 35. javascript的typeof返回哪些数据类型?

答案：string,boolean,number,undefined,function,object,symbol

#### 36. 例举3种强制类型转换和2种隐式类型转换?

答案：强制（parseInt,parseFloat,number）
隐式（== ===）

#### 37. split() 和 join() 的区别？

答案：前者是将字符串切割成数组的形式，后者是将数组转换成字符串

#### 38. 数组方法pop() push() unshift() shift()的区别？

答案：push()尾部添加 pop()尾部删除
unshift()头部添加 shift()头部删除

#### 39. IE和标准下有哪些兼容性的写法?

答案：

```js

var ev = ev || window.event
document.documentElement.clientWidth || document.body.clientWidth
Var target = ev.srcElement||ev.target
```

#### 40. ajax请求的时候get 和post方式的区别?

答案：
一个在url后面 ，一个放在虚拟载体里面
get有大小限制(只能提交少量参数)
安全问题
应用不同 ，请求数据和提交数据

#### 41. call和apply的区别?

答案：

```js

Object.call(this,obj1,obj2,obj3)
Object.apply(this,arguments)
```

#### 42. ajax请求时，如何解析json数据?

答案：使用JSON.parse

#### 44. 事件委托是什么?

答案: 利用事件冒泡的原理，让自己的所触发的事件，让他的父元素代替执行！

#### 45. 闭包是什么，有什么特性，对页面有什么影响?

答案：闭包就是能够读取其他函数内部变量的函数,使得函数不被GC回收，如果过多使用闭包，容易导致内存泄露

#### 46. 如何阻止事件冒泡?

答案：ie:阻止冒泡ev.cancelBubble = true;非IE ev.stopPropagation();

#### 47. 如何阻止默认事件?

答案：(1)return false；(2) ev.preventDefault();

#### 48. 添加 删除 替换 插入到某个接点的方法?

答案：
1）创建新节点
createElement() //创建一个具体的元素
createTextNode() //创建一个文本节点

2）添加、移除、替换、插入
appendChild() //添加
removeChild() //移除
replaceChild() //替换
insertBefore() //插入

3）查找
getElementsByTagName() //通过标签名称
getElementsByName() //通过元素的Name属性的值
getElementById() //通过元素Id，唯一性

#### 49. 解释jsonp的原理，以及为什么不是真正的ajax

答案：动态创建script标签，回调函数
Ajax是页面无刷新请求数据操作

#### 50. document load 和document ready的区别

答案：document.onload 是在结构和样式,外部js以及图片加载完才执行js
document.ready是dom树创建完成就执行的方法，原生种没有这个方法，jquery中有 $().ready(function)

#### 51. ”\==”和 “ === ”的不同?

答案：前者会自动转换类型,再判断是否相等
后者不会自动类型转换，直接去比较

#### 52. 函数声明与函数表达式的区别？

在Javscript中，解析器在向执行环境中加载数据时，对函数声明和函数表达式并非是一视同仁的，解析器会率先读取函数声明，并使其在执行任何代码之前可用（可以访问），至于函数表达式，则必须等到解析器执行到它所在的代码行，才会真正被解析执行。

#### 53. 对作用域上下文和this的理解，看下列代码：

```js

var User = {
 count: 1,
 getCount: function() {
  return this.count;
 }
};
console.log(User.getCount()); // what?
var func = User.getCount;
console.log(func()); // what?
```

问两处console输出什么？为什么？
答案:是1和undefined。
　　func是在window的上下文中被执行的，所以不会访问到count属性。

#### 54. 看下面代码，给出输出结果。

```js

for(var i = 1; i <= 3; i++){  //建议使用let 可正常输出i的值
  setTimeout(function(){
      console.log(i);   
  },0); 
};
```

答案：4 4 4。

原因：Javascript事件处理器在线程空闲之前不会运行。

#### 55. 当一个DOM节点被点击时候，我们希望能够执行一个函数，应该怎么做?

```js

box.onlick= function(){}
 
box.addEventListener("click",function(){},false);
 
<button onclick="xxx()"></button>
```

### 56. Javascript的事件流模型都有什么?

“事件冒泡”：事件开始由最具体的元素接受，然后逐级向上传播

“事件捕捉”：事件由最不具体的节点先接收，然后逐级向下，一直到最具体的

“DOM事件流”：三个阶段：事件捕捉，目标阶段，事件冒泡

#### 57. 看下列代码,输出什么?解释原因。

```js

var a = null;
alert(typeof a);
```

答案：object
解释：null是一个只有一个值的数据类型，这个值就是null。表示一个空指针对象，所以用typeof检测会返回”object”。

#### 58. 判断字符串以字母开头，后面可以是数字，下划线，字母，长度为6-30

var reg=/1\w{5,29}$/;

### 59. 回答以下代码，alert的值分别是多少？

```js

<script>
     var a = 100;  
     function test(){  
        alert(a);  
        a = 10;  //去掉了var 就变成定义了全局变量了
        alert(a);  
}  
test();
alert(a);
</script>
```

正确答案是： 100， 10， 10

#### 60. javaScript的2种变量范围有什么不同？

全局变量：当前页面内有效

局部变量：函数方法内有效

#### 61. null和undefined的区别？

null是一个表示”无”的对象，转为数值时为0；undefined是一个表示”无”的原始值，转为数值时为NaN。

当声明的变量还未被初始化时，变量的默认值为undefined。 null用来表示尚未存在的对象

undefined表示”缺少值”，就是此处应该有一个值，但是还没有定义。典型用法是：

（1）变量被声明了，但没有赋值时，就等于undefined。

（2）调用函数时，应该提供的参数没有提供，该参数等于undefined。

（3）对象没有赋值的属性，该属性的值为undefined。

（4）函数没有返回值时，默认返回undefined。

null表示”没有对象”，即该处不应该有值。典型用法是：

（1） 作为函数的参数，表示该函数的参数不是对象。

（2） 作为对象原型链的终点。

#### 62. new操作符具体干了什么呢?

1、创建一个空对象，并且 this 变量引用该对象，同时还继承了该函数的原型。

2、属性和方法被加入到 this 引用的对象中。

3、新创建的对象由 this 所引用，并且最后隐式的返回 this 。

#### 63. js延迟加载的方式有哪些？

defer和async、动态创建DOM方式（创建script，插入到DOM中，加载完毕后callBack）、按需异步载入js

#### 64. Flash、Ajax各自的优缺点，在使用中如何取舍？

Flash ajax对比

(1)Flash适合处理多媒体、矢量图形、访问机器；对CSS、处理文本上不足，不容易被搜索。

(2)ajax对CSS、文本支持很好，支持搜索；多媒体、矢量图形、机器访问不足。

```js

共同点：与服务器的无刷新传递消息、用户离线和在线状态、操作DOM
```

#### 65. 写一个获取非行间样式的函数

```js


function getStyle(obj,attr) {
 
if(obj.currentStyle) {
 
return obj.currentStyle[attr];
 
}else{
 
getComputedStyle(obi,false)[attr] 
 
}
 
}
```

#### 66. 希望获取到页面中所有的checkbox怎么做？(不使用第三方框架)

```js

var inputs = document.getElementsByTagName("input");//获取所有的input标签对象
var checkboxArray = [];//初始化空数组，用来存放checkbox对象。
for(var i=0;i<inputs.length;i++){
  var obj = inputs[i];
  if(obj.type=='checkbox'){
     checkboxArray.push(obj);
  }
}
```

#### 67. 写一个function，清除字符串前后的空格。（兼容所有浏览器）

```js

String.prototype.trim= function(){
 
return this.replace(/^\s+/,"").replace(/\s+$/,"");
 
}
```

#### 68. javascript语言特性中，有很多方面和我们接触的其他编程语言不太一样,请举例

javascript语言实现继承机制的核心就是 1 (原型)，而不是Java语言那样的类式继承。Javascript解析引擎在读取一个Object的属性的值时，会沿着 2 (原型链)向上寻找，如果最终没有找到，则该属性值为 3 undefined；如果最终找到该属性的值，则返回结果。与这个过程不同的是，当javascript解析引擎执行“给一个Object的某个属性赋值”的时候，如果当前Object存在该属性，则改写该属性的值，如果当前的Object本身并不存在该属性，则赋值该属性的值。

#### 69. Cookie在客户机上是如何存储的

Cookies就是服务器暂存放在你的电脑里的文本文件，好让服务器用来辨认你的计算机。当你在浏览网站的时候，Web服务器会先送一小小资料放在你的计算机上，Cookies 会帮你在网站上所打的文字或是一些选择都记录下来。当下次你再访问同一个网站，Web服务器会先看看有没有它上次留下的Cookies资料，有的话，就会依据Cookie里的内容来判断使用者，送出特定的网页内容给你。

#### 70. 如何获取javascript三个数中的最大值和最小值？

Math.max(a,b,c);//最大值

Math.min(a,b,c)//最小值

#### 71. javascript是面向对象的，怎么体现javascript的继承关系？

使用prototype原型来实现。

#### 72. form中的input可以设置为readonly和disable，请问2者有什么区别？

readonly不可编辑，但可以选择和复制；值可以传递到后台
disabled不能编辑，不能复制，不能选择；值不可以传递到后台

#### 73. 程序中捕获异常的方法？

```js

try{
 
}catch(e){
 
}finally{
 
}
```

#### 74. Ajax原理

(1)创建对象

var xhr = new XMLHttpRequest();

(2)打开请求

xhr.open(‘GET’, ‘example.txt’, true);

(3)发送请求

xhr.send(); 发送请求到服务器

(4)接收响应

xhr.onreadystatechange =function(){}

(1)当readystate值从一个值变为另一个值时，都会触发readystatechange事件。

(2)当readystate==4时，表示已经接收到全部响应数据。

(3)当status ==200时，表示服务器成功返回页面和数据。

(4)如果(2)和(3)内容同时满足，则可以通过xhr.responseText，获得服务器返回的内容。

#### 75. 解释什么是Json:

(1)JSON 是一种轻量级的数据交换格式。

(2)JSON 独立于语言和平台，JSON 解析器和 JSON 库支持许多不同的编程语言。

(3)JSON的语法表示三种类型值，简单值(字符串，数值，布尔值，null),数组，对象

#### 76. js中的3种弹出式消息提醒（警告窗口，确认窗口，信息输入窗口）的命令式什么？

alert
confirm
prompt

#### 77. 以下代码执行结果

```js

var uname = 'jack'
function change() {
    alert(uname) // ?
    var uname = 'lily'
    alert(uname)  //?
}
change()
```

分别alert出 undefined，lily，（变量声明提前问题）

#### 78. 浏览器的滚动距离：

可视区域距离页面顶部的距离

scrollTop=document.documentElement.scrollTop||document.body.scrollTop

#### 79. 可视区的大小：

(1)innerXXX（不兼容ie）

```js

window.innerHeight 可视区高度，包含滚动条宽度
 
window.innerWidth  可视区宽度，包含滚动条宽度
```

(2)document.documentElement.clientXXX(兼容ie)

```js

document.documentElement.clientWidth可视区宽度，不包含滚动条宽度
 
document.documentElement.clientHeight可视区高度，不包含滚动条宽度
```

#### 80. 节点的种类有几种，分别是什么？

(1)元素节点：nodeType ===1;

(2)文本节点：nodeType ===3;

(3)属性节点：nodeType ===2;

#### 81. innerHTML和outerHTML的区别

innerHTML(元素内包含的内容）

outerHTML(自己以及元素内的内容）

#### 82. offsetWidth offsetHeight和clientWidth clientHeight的区别

(1)offsetWidth （content宽度+padding宽度+border宽度）

(2)offsetHeight（content高度+padding高度+border高度）

(3)clientWidth（content宽度+padding宽度）

(4)clientHeight（content高度+padding高度）

#### 83. 闭包的好处

(1)希望一个变量长期驻扎在内存当中(不被垃圾回收机制回收)

(2)避免全局变量的污染

(3)私有成员的存在

(4)安全性提高

#### 84. 冒泡排序算法

冒泡排序：

```js


var array = [5, 4, 3, 2, 1];
var temp = 0;
for (var i = 0; i <array.length; i++){
for (var j = 0; j <array.length - i; j++){
if (array[j] > array[j + 1]){
temp = array[j + 1];
array[j + 1] = array[j];
array[j] = temp;
 }
}
}
```

#### 85.不使用循环，创建一个长度为100的数组，并且每个元素的值等于它的小标。

new Array(100).fill(0).map((_, c) => c)

#### 86. 给定一个数组例如[1,3,4,6,7] ，再给定一个目标数，例如9。 写一个算法找出两个数他们相加等于目标数，返回他们在数组中的位置。

```js


function func(arr,target){
        var obj = {};
            for(var i = 0; i < arr.length; i++){
                var item = arr[i];
                if(obj[item] === undefined){
                    var x = target - item;
                    obj[x] = i;
                }else{
                    return [obj[item],i];  
                }
            }
            return null;
        }
         
        console.log(func([1,3,7,6,9,11],9))
```

------

1.a-zA-Z
