# 浏览器环境

	<!-- 加载Traceur编译器 -->
	<script src="http://google.github.io/traceur-compiler/bin/traceur.js" type="text/javascript"></script>
	<!-- 将Traceur编译器用于网页 -->
	<script src="http://google.github.io/traceur-compiler/src/bootstrap.js" type="text/javascript"></script>
	<!-- 打开实验选项，否则有些特性可能编译不成功 -->
	<script>
        traceur.options.experimental = true;
	</script>
 
	<script type="module">
  	class Calc {
    	constructor(){
        	console.log('Calc constructor');
    	}
    	add(a, b){
      		return a + b;
    	}
    }
 
  	var c = new Calc();
  	console.log(c.add(4,5));
	</script>
	
# let

* 声明变量的，只在代码块中有效 

```
{
  let a = 10;
  var b = 1;
}

a // ReferenceError: a is not defined.
b // 1
```

* 不存在变量提升

```
function do_something() {
  console.log(foo); // ReferenceError
  let foo = 2;
}
```

* 暂时性死区

```
var tmp = 123;

if (true) {
  tmp = 'abc'; // ReferenceError
  let tmp;
}
```

暂时性死区的本质就是，只要一进入当前作用域，所要使用的变量就已经存在了，但是不可获取，只有等到声明变量的那一行代码出现，才可以获取和使用该变量。

* 不允许重复声明

```
// 报错
function () {
  let a = 10;
  var a = 1;
}

// 报错
function () {
  let a = 10;
  let a = 1;
}

function func(arg) {
  let arg; // 报错
}
```
# 块级作用域（相当于立即执行函数）

```
// IIFE写法
(function () {
  var tmp = ...;
  ...
}());

// 块级作用域写法
{
  let tmp = ...;
  ...
}
```

# 块级作用域es5和es6比较

* es5

```
for (var i = 0; i < 5; i++){
  console.log(s[i]);
}
console.log(i); // 5
```
* es6

```
function f1() {
  let n = 5;
  if (true) {
    let n = 10;
  }
  console.log(n); // 5
}
```

# const

* 声明是常量，不可更改
* 块级作用域内有效

# 全局属性

ES6规定，var命令和function命令声明的全局变量，属于全局对象的属性；let命令、const命令、class命令声明的全局变量，不属于全局对象的属性

```
var a = 1;
// 如果在node环境，可以写成global.a
// 或者采用通用方法，写成this.a
window.a // 1

let b = 1;
window.b // undefined
```


