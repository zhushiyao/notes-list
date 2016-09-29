#正在学习使用ES6，所有整理一份ES6的常用API文档，方便使用。
####学习文档主要摘自阮一峰的ECMAScript 6入门[http://es6.ruanyifeng.com/#README](http://es6.ruanyifeng.com/#README)

####目前在项目中个人比较常用的语法
let 声明
```js
let a = 1;
```
for循环遍历
```js
for(var i=0;i<list.length-1;i++){
  console.log(list[i]);
}
//等价于
for(let item of list){
  console.log(item);
}
```
function缩写与=>的使用
```js
var fn = function() {
  console.log('嗨');
}
//等价于
let fn = () =>{
}
//这种写法比较常用于对象声明与请求的回调函数中，使用=>可以提升作用域，使this等价清晰
var _this = this;
ajax.then(json =>{
  _this.list = json;
);
//等价于
ajax.then(json=>{
  this.list = json;
);
```
## 说明
ECMAScript 6.0（以下简称ES6）是JavaScript语言的下一代标准，已经在2015年6月正式发布了。它的目标，是使得JavaScript语言可以用来编写复杂的大型应用程序，成为企业级开发语言。
标准的制定者有计划，以后每年发布一次标准，使用年份作为版本。因为ES6的第一个版本是在2015年发布的，所以又称ECMAScript 2015（简称ES2015）。
2016年6月，小幅修订的《ECMAScript 2016 标准》（简称 ES2016）如期发布。由于变动非常小（只新增了数组实例的includes方法和指数运算符），因此 ES2016 与 ES2015 基本上是同一个标准，都被看作是 ES6。根据计划，2017年6月将发布 ES2017。
***

##let命令
ES6新增了let命令，用来声明变量。它的用法类似于var，具有以下几个特点。
let所声明的变量，只在let命令所在的代码块内有效
```js
 {
    let a = 666;
    var b = 1;
  }

  a // 报错: a is not defined.
  b // 1
```

不存在变量提升<br> 
let不像var那样会发生“变量提升”现象。所以，变量一定要在声明后使用，否则报错。
```js
console.log(foo); // 输出undefined
console.log(bar); // 报错ReferenceError

var foo = 2;
let bar = 2;
````

暂时性死区<br>
只要块级作用域内存在let命令，它所声明的变量就“绑定”（binding）这个区域，不再受外部的影响。
```js
var tmp = 123;

if (true) {
  tmp = 'abc'; // ReferenceError
  let tmp;
}
```

不允许重复声明<br>
```js
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
```

###块级作用域
下面的函数有两个代码块，都声明了变量n，运行后输出5。这表示外层代码块不受内层代码块的影响。如果使用var定义变量n，最后输出的值就是10。
```js
 function f1() {
  let n = 5;
  if (true) {
    let n = 10;
  }
  console.log(n); // 5
}
```
ES6允许块级作用域的任意嵌套
```js
{{{{{let insane = 'Hello World'}}}}};
```

###const命令
const声明一个只读的常量。一旦声明，常量的值就不能改变。
```js
const PI = 3.1415;
PI // 3.1415

PI = 3;
// TypeError: Assignment to constant variable.
```
const声明的变量不得改变值，这意味着，const一旦声明变量，就必须立即初始化，不能留到以后赋值。
```js
const foo;
// SyntaxError: Missing initializer in const declaration
```
const的作用域与let命令相同：只在声明所在的块级作用域内有效
```js
if (true) {
  const MAX = 5;
}
MAX // Uncaught ReferenceError: MAX is not defined
```
const命令声明的常量也是不提升，同样存在暂时性死区，只能在声明的位置后面使用。
```js
if (true) {
  console.log(MAX); // ReferenceError
  const MAX = 5;
}
```
const声明的常量，也与let一样不可重复声明。
```js
var message = "Hello!";
let age = 25;

// 以下两行都会报错
const message = "Goodbye!";
const age = 30;
```
###数组的解构赋值
ES6允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构（Destructuring）。
```js
var a = 1;
var b = 2;
var c = 3;
```
ES6允许写成下面这样。
```js
var [a, b, c] = [1, 2, 3];
```
上面代码表示，可以从数组中提取值，按照对应位置，对变量赋值。

<br>
解构赋值不仅适用于var命令，也适用于let和const命令。
```js
var [v1, v2, ..., vN ] = array;
let [v1, v2, ..., vN ] = array;
const [v1, v2, ..., vN ] = array;
```
###默认值
解构赋值允许指定默认值。
```js
var [foo = true] = [];
foo // true

[x, y = 'b'] = ['a']; // x='a', y='b'
[x, y = 'b'] = ['a', undefined]; // x='a', y='b'
```
注意，ES6内部使用严格相等运算符（===），判断一个位置是否有值。所以，如果一个数组成员不严格等于undefined，默认值是不会生效的。

```js
var [x = 1] = [undefined];
x // 1

var [x = 1] = [null];
x // null
```
上面代码中，如果一个数组成员是null，默认值就不会生效，因为null不严格等于undefined。


###对象的解构赋值
解构不仅可以用于数组，还可以用于对象。
```js
var { foo, bar } = { foo: "aaa", bar: "bbb" };
foo // "aaa"
bar // "bbb"
```
对象的解构与数组有一个重要的不同。数组的元素是按次序排列的，变量的取值由它的位置决定；而对象的属性没有次序，变量必须与属性同名，才能取到正确的值。
```js
ar { bar, foo } = { foo: "aaa", bar: "bbb" };
foo // "aaa"
bar // "bbb"

var { baz } = { foo: "aaa", bar: "bbb" };
baz // undefined
```
上面代码的第一个例子，等号左边的两个变量的次序，与等号右边两个同名属性的次序不一致，但是对取值完全没有影响。第二个例子的变量没有对应的同名属性，导致取不到值，最后等于undefined。
<br>
如果变量名与属性名不一致，必须写成下面这样。
```js
var { foo: baz } = { foo: 'aaa', bar: 'bbb' };
baz // "aaa"

let obj = { first: 'hello', last: 'world' };
let { first: f, last: l } = obj;
f // 'hello'
l // 'world'
```


###函数参数的默认值
在ES6之前，不能直接为函数的参数指定默认值，只能采用变通的方法
```js
function log(x, y) {
  y = y || 'World';
  console.log(x, y);
}

log('Hello') // Hello World
log('Hello', 'China') // Hello China
log('Hello', '') // Hello World

```
ES6允许为函数的参数设置默认值，即直接写在参数定义的后面。
```js
function log(x, y = 'World') {
  console.log(x, y);
}

log('Hello') // Hello World
log('Hello', 'China') // Hello China
log('Hello', '') // Hello
```

###与解构赋值默认值结合使用
参数默认值可以与解构赋值的默认值，结合起来使用。
```js
function foo({x, y = 5}) {
  console.log(x, y);
}

foo({}) // undefined, 5
foo({x: 1}) // 1, 5
foo({x: 1, y: 2}) // 1, 2
foo() // TypeError: Cannot read property 'x' of undefined
```
###函数的length属性
指定了默认值以后，函数的length属性，将返回没有指定默认值的参数个数。也就是说，指定了默认值后，length属性将失真。
```js
(function (a) {}).length // 1
(function (a = 5) {}).length // 0
(function (a, b, c = 5) {}).length // 2
```
上面代码中，length属性的返回值，等于函数的参数个数减去指定了默认值的参数个数。比如，上面最后一个函数，定义了3个参数，其中有一个参数c指定了默认值，因此length属性等于3减去1，最后得到2

###对象的扩展
属性的简洁表示法<br>
ES6允许直接写入变量和函数，作为对象的属性和方法。这样的书写更加简洁。
```js
var foo = 'bar';
var baz = {foo};
baz // {foo: "bar"}

// 等同于
var baz = {foo: foo};
```
上面代码表明，ES6允许在对象之中，只写属性名，不写属性值。这时，属性值等于属性名所代表的变量。下面是另一个例子。
```js
function f(x, y) {
  return {x, y};
}

// 等同于

function f(x, y) {
  return {x: x, y: y};
}

f(1, 2) // Object {x: 1, y: 2}
```
除了属性简写，方法也可以简写。
```js
var o = {
  method() {
    return "Hello!";
  }
};

// 等同于

var o = {
  method: function() {
    return "Hello!";
  }
};
```



---
###未完、待续
---





