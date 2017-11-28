### js面向对象常规写法

```javascript
function Person(name,age){
    this.name=name;
    this.age=age;
}
Person.prototype.say=function(){
    alert('我的名字叫：'+this.name+'我的年龄'+this.age)
    }
var p=new Person('ycy',27);
p.say();
```

### js的继承常规写法

```javascript
function Person(name,age){
    this.name=name;
    this.age=age;
}
Person.prototype.say=function(){
    alert('我的名字叫：'+this.name+'我的年龄'+this.age)
}
    
function Chinese(name,age,aihao){
    this.aihao=aihao;
    Person.call(this,name,age);
    }
Chinese.prototype=new Person();
Chinese.prototype.sayAihao=function(){
    alert('我的爱好是：'+this.aihao)
    }
var c=new Chinese('ycy',27,'gongfu');
c.sayAihao();
```

### 跨域问题

    不在同一个域或者子域下都要跨域，比如你在 www.a.com下面要去访问 www.b.com或者s.a.com都要跨域。
    解决办法：通过js标签加载 如：
    	<script type=”text/javascript” src=”http://act.hi.baidu.com/widget/recommend”><script>
    Jsonp跨域原理：JS虽然不支持跨域，但是js都可以用js标签加载跨域的js，jsonp利用这个原理，相当于将数据封装到了一个js文件里面，用js标签加载过来后解析执行这个js得到数据。
    
### 什么是闭包?    
    
    定义:当在一个函数内定义另外一个函数就会产生闭包.
    特点：闭包的局部变量可以在函数执行结束后仍然被函数外的代码访问。
    作用：一个是可以读取函数内部的变量，另一个就是让这些变量的值始终保持在内存中。
    用法：函数作为返回值，函数作为参数传递，还有立即调用函数表达式。
    缺点：由于闭包会使得函数中的变量都被保存在内存中，内存消耗很大，所以不能滥用闭包，否则会造成网页的性能问题，在IE中可能导致内存泄露。解决方法是，在退出函数之前，将不使用的局部变量全部删除。
    涉及到的JS特性：作用域链,垃圾(内存)回收机制,函数嵌套,；
    作用域链就是函数在定义的时候创建的,用于寻找使用到的变量值的一个索引;
    function a() { 
         var i = 0; 
        function b() { alert(++i); } 
        return b;}
    var c = a();
    c();
    当函数a的内部函数b被函数a外的一个变量引用的时候，就创建了一个闭包。
    闭包内的微观世界(帮助理解)
　　			如果要更加深入的了解闭包以及函数a和嵌套函数b的关系，我们需要引入另外几个概念：函数的执行环境(excution context)、活动对象(call object)、作用域(scope)、作用域链(scope chain)。以函数a从定义到执行的过程为例阐述这几个概念。
        当定义函数a的时候，js解释器会将函数a的作用域链(scope chain)设置为定义a时a所在的“环境”，如果a是一个全局函数，则scope chain中只有window对象。
        当执行函数a的时候，a会进入相应的执行环境(excution context)。
        在创建执行环境的过程中，首先会为a添加一个scope属性，即a的作用域，其值就为第1步中的scope chain。即a.scope=a的作用域链。
        然后执行环境会创建一个活动对象(call object)。活动对象也是一个拥有属性的对象，但它不具有原型而且不能通过JavaScript代码直接访问。创建完活动对象后，把活动对象添加到a的作用域链的最顶端。此时a的作用域链包含了两个对象：a的活动对象和window对象。
        下一步是在活动对象上添加一个arguments属性，它保存着调用函数a时所传递的参数。
        最后把所有函数a的形参和内部的函数b的引用也添加到a的活动对象上。在这一步中，完成了函数b的的定义，因此如同第3步，函数b的作用域链被设置为b所被定义的环境，即a的作用域。
        到此，整个函数a从定义到执行的步骤就完成了。此时a返回函数b的引用给c，又函数b的作用域链包含了对函数a的活动对象的引用，也就是说b可以访问到a中定义的所有变量和函数。函数b被c引用，函数b又依赖函数a，因此函数a在返回后不会被GC回收。
        当函数b执行的时候亦会像以上步骤一样。因此，执行时b的作用域链包含了3个对象：b的活动对象、a的活动对象和window对象，如下图所示：
        当在函数b中访问一个变量的时候，搜索顺序是：
        先搜索自身的活动对象，如果存在则返回，如果不存在将继续搜索函数a的活动对象，依次查找，直到找到为止。
        如果函数b存在prototype原型对象，则在查找完自身的活动对象后先查找自身的原型对象，再继续查找。这就是Javascript中的变量查找机制。
        如果整个作用域链上都无法找到，则返回undefined。
        小结，本段中提到了两个重要的词语：函数的定义与执行。文中提到函数的作用域是在定义函数时候就已经确定，而不是在执行的时候确定（参看步骤1和3）。
        另一种类型的闭包叫做立即调用函数表达式(immediately-invoked function expression IIFE)，无非是一个在window上下文中的自调用匿名函数(self-invoked anonymous function)。


    
### 作用域问题    
    
    作用域是基于函数(function-based)而上下文是基于对象(object-based)。作用域是和每次函数调用时变量的访问有关，并且每次调用都是独立的。上下文总是关键字 this 的值，是调用当前可执行代码的对象的引用。
    目前javascript不支持块级作用域，块级作用域指在if语句，switch语句，循环语句等语句块中定义变量，这意味着变量不能在语句块之外被访问。当前任何在语句块中定义的变量都能在语句块之外访问。然而，这种情况很快会得到改变，let 关键字已经正式添加到ES6规范。
    用它来代替var关键字可以将局部变量声明为块级作用域。当调用一个函数时通过new的操作符创建一个对象的实例。当以这种方式调用时，this 的值将被设置为新创建的实例：
    function foo(){
         alert(this);}
    foo() // window
    new foo() // foo
    当调用一个未绑定函数，this 将被默认设置为全局上下文(global context) 或window对象(如果在浏览器中)。然而如果函数在严格模式下被执行(“use strict”)，this的值将被默认设置为undefined。

### 预解析(也就是执行上下文环境)

    预解析过程完成的工作：变量、函数表达式——声明，默认赋值为undefined；	this——赋值；	函数声明——赋值；这三种数据的准备情况我们称之为“执行上下文”或者“执行上下文环境”。
    函数每被调用一次，都会产生一个新的执行上下文环境。因为不同的调用可能就会有不同的参数，在执行代码之前，把将要用到的所有的变量都事先拿出来，有的直接赋值了，有的先用undefined占个空。。
    当函数调用完成时，这个上下文环境以及其中的数据都会被消除，再重新回到全局上下文环境。处于活动状态的执行上下文环境只有一个。这就是一个压栈出栈的过程——执行上下文栈。	
    javascript除了全局作用域之外，只有函数可以创建的作用域。作用域只是一个“地盘”，一个抽象的概念，其中没有变量。要通过作用域对应的执行上下文环境来获取变量的值。同一个作用域下，不同的调用会产生不同的执行上下文环境，继而产生不同的变量的值。
    所以，作用域中变量的值是在执行过程中产生的确定的，而作用域却是在函数创建时就确定了。
    函数在定义的时候（不是调用的时候），就已经确定了函数体内部自由变量的作用域，这就是所谓的“静态作用域”。


### 原型链    
    
    a的原型是b，b的原型是c，那么a继承b和c的属性和方法；如果a重新设置的方法或属性与b和c中的属性重名，那么a的这个属性覆盖原型链中的属性，但是并不改变原型链的属性。
    
### js的GC回收机制

    js会自动进行内存回收，不用的变量会自动抛弃，实际上一般理解为局部变量在函数执行完后即被抛弃，但是也有一些例外。比如：闭包内部返回局部变量就不会被回收。
    
### 写出以下代码的运行结果？
    alert(typeof null) ->	object
    alert(typeof undefined) -> undefined
    alert(typeof NaN) ->	number
    alert(null==undefined) ->	true
    alert(NaN==NaN)  -> false
    var str=’123abc’; 	NaN
    alert(typeof str++);  -> number
    alert(str)  -> NaN
    alert(Number(null))    -> 0
    alert(Number([]))    -> 0
    [] == false; // true空数组本身是对象不是空，但在比较的时候他是false;
    [] == ![];   // true空数组取反转成了boolean，是false;
    
### 如何判断某变量是否为数组数据类型？
    方法一：obj instanceof Array 在某些IE版本中不正确
    方法二：判断其是否具有“数组性质”，如sort
    方法三：在ECMA Script5中定义了新方法Array.isArray()
    
### 正则表达式构造函数var reg=new RegExp(“xxx”)与正则表达字面量var reg=//有什么不同？
    
    当使用RegExp()构造函数的时候，不仅需要转义引号（即\”表示”），并且还需要双反斜杠（即\\表示一个\）。使用正则表达字面量的效率更高。
    
### 实现一个函数clone，可以对JavaScript中的5种主要的数据类型进行值复制（主要是对象和数组赋值与其他类型不一样）
```javascript
Object.prototype.clone = function(){
    var o = this.constructor === Array ? [] : {};
    for(var e in this){
            o[e] = typeof this[e] === "object" ? this[e].clone() : this[e]; }
     return o;
}
       		 
```

### 函数声明与函数表达式的区别？

    在js中，解析器在向执行环境中加载数据时，对函数声明和函数表达式并非是一视同仁的，解析器会率先读取函数声明，
    并使其在执行任何代码之前可用（可以访问），至于函数表达式，则必须等到解析器执行到它所在的代码行，才会真正被解析执行。

### 定义一个log方法，让它可以代理console.log的方法

```javascript
function log(){
    console.log.apply(console, arguments);
  };
```

### 在Javascript中什么是伪数组？如何将伪数组转化为标准数组？

    伪数组（类数组）：无法直接调用数组方法或期望length属性有什么特殊的行为，但仍可以对真正数组遍历方法来遍历它们。典型的是函数的argument参数，
    还有像调用getElementsByTagName,document.childNodes之类的,它们都返回NodeList对象都属于伪数组。
    可以使用Array.prototype.slice.call(fakeArray)将数组转化为真正的Array对象。
    
### 浏览器嗅探:/android/.test(navigation.userAgent)    
    
### 原生JS的window.onload与Jquery的$(document).ready(function(){})有什么不同？如何用原生JS实现Jq的ready方法？

```javascript
function ready(fn){
  if(document.addEventListener) {    
      document.addEventListener('DOMContentLoaded', function() {
          //注销事件, 避免反复触发
          document.removeEventListener('DOMContentLoaded',arguments.callee, false);
          fn();            //执行函数
      }, false);
  }else if(document.attachEvent) {
      document.attachEvent('onreadystatechange', function() {
         if(document.readyState == 'complete') {
             document.detachEvent('onreadystatechange', arguments.callee);
             fn();
         } 
      }); 
  }
};
```

### javascript的本地对象，内置对象和宿主对象
    本地对象为array obj regexp等可以new实例化
    内置对象为gload Math 等不可以实例化的
    宿主为浏览器自带的document,window等.
    
### 去除首尾字符并整理内部空格    
    str = str.split(/\s+/g).join(' ').replace(/^\s+|\s+$/g,'');
    
### 加密解密
```
html部分
<body>
<input type="text" id="tx1" />
<input type="button" value="加密" id="btn1" />
<input type="button" value="解密" id="btn2" />
<input type="text" id="tx2" />
</body>
```
```javascript
window.onload = function(){
	var oT1 = document.getElementById('tx1');
	var oBtn1 = document.getElementById('btn1');
	var oBtn2 = document.getElementById('btn2');
	var oT2 = document.getElementById('tx2');
	oBtn1.onclick = function(){
		var str = oT1.value;
		var str2 = '';
		for(var i=0;i<str.length;i++){
			str2+= String.fromCharCode(str.charCodeAt(i)+123);}
		oT2.value = str2;};
	oBtn2.onclick = function(){
		var str2 = oT2.value;
		var str = '';
		for(var i=0;i<str2.length;i++){
			str+= String.fromCharCode(str2.charCodeAt(i)-123);}
		oT1.value = str;
	};
};
```



