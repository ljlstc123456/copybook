# let 和 const 命令 {#let-const-}

## 1、let命令：

### 基本用法

es6新增了let语法命令，用来声明变量。他的用法类似于var，但是所声明的变量，只在let命令所在的代码块内有效。

```
{
    let a = 10 ;
    var b =1;
}
a //ReferneceError:a is not defined.
b //1
```

上面的代码在代码块之中，分别用let和var声明了两个变量。然后在代码块之外调用这两个变量，结果let声明的变量报错，var声明的变量返回了正确的值。这表明，let声明的变量只在它所在的代码块有效。

for循环计数器，就很适合使用let命令。

```
for(let i = 0; i < 10; i++){
    //...
}
console.log(i) ;
//ReferenceError:i is not defined
```

上面代码中，计数器i只在循环体内有效，在循环体外引用就报错。

下面的代码如果使用var，最后输出的是10。

```
var a = [] ;
for(var i = 0; i < 10; i++){
    a[i] = function(){
        console.log(i) ;
    }
}

a[6]() ;//10

//如果不用es6的解决方法是闭包
var a = [] ;
for(var i = 0; i < 10; i++){
    a[i] = (function(i){
       return function(){ console.log(i) };
    })(i) ;
}

a[6]() ;//6
```

上面的代码中，变量i是var命令声明的，在全局范围内都有效，所以全局只有一个变量i。每一次循环，变量i的值都会发生改变，而循环内被赋给数组a的函数内部的console.log\(i\)，里面的i指向的就是全局的i。也就是说，所有数组a的成员里面的i，指向的都是同一个i，导致运行时输出的是最后一轮的i的值，也就是10。

如果使用let，声明的变量仅在块级作用域内有效，最后输出的是6。

```
var a = [] ;
for(let i = 0; i < 10; i++) {
    a[i] = function() {
        console.log(i);
    } ;
}
a[6]();//6
```

上面的代码中，变量i是let声明的，当前i只在本轮循环有效，所以每次循环的i其实都是一个新的变量，所以最后输出的是6。你可能会问，如果每一轮循环的变量i都是重新声明的，那它怎么知道上一轮循环的值，从而计算出本轮循环的值？这是因为javascript引擎内部会记住上一轮循环的值，初始化本轮的变量i时，就在上一轮循环的基础上进行计算。

另外，for循环还有一个特别之处，就是设置循环变量的那部分是一个父作用域，而循环体内部是一个子作用域。

```
for(let i=0;i<3;i++){
    let i = 'abc' ;
    console.log(i) ;
}
//abc
//abc
//abc
```

上面的代码正确运行，输出了3次abc。这表明函数内部的变量i与循环体i不在同一个作用域，

有各自单独的作用域。

### 不存在变量提升

var命令会发生"变量提升"现象，即变量可以在声明之前使用，值为undefined。这种现象多多少少有些奇怪，按照一般的逻辑，变量应该在声明语句之后才可以使用。

为了纠正这一种现象，let命令改变了语法行为，它声明的变量一定要在声明后使用，否则报错。

```
// var的情况
console.log(foo) ; //输出undefined
var foo = 2;

// let的情况
console.log(bar); //报错ReferenceError
let bar = 2 ;
```

上面的代码中，变量foo用var命令声明，会发生变量提升，即脚本开始运行时，变量foo已经存在了，但是没有值，所以会输出undefined。变量bar用let命令声明，不会发生变量提升。这表示在声明它之前，变量bar是不存在的，这时如果用到它，就会抛出一个错误。

### 暂时性死区

只要块级作用域内存在let命令，它声明的变量就"绑定"（binding）这个区域，不再受外部的影响。

```
var tmp = 123;
if(true){
    tmp = 'abc';//ReferenceError
    let tmp;
}
```

上面代码中，存在全局变量tmp，但是块级作用域内let又声明了一个局部变量tmp，导致后者绑定这个块级作用域，所以在let声明变量前，对tmp赋值会报错。

ES6明确规定，如果区块中存在let和const命令，这个区块对这些命令声明的变量，从一开始就形成了封闭作用域。凡是在声明之前就使用这些变量，就会报错。

总之，在代码块内，使用let命令声明变量之前，该变量是不可用的。这在语法上，称为"暂时性死区"（temporal dead zone，简称TDZ）。

```
if(true){
    // TDZ开始
    tmp = 'abc' ;// ReferenceError
    console.log(tmp) ;// ReferenceError

    let tmp; // TDZ 结束

    console.log(tmp) //undefined

    tem = 123;
    console.log(tmp) ;123
}
```

上面代码中，在let命令声明变量tmp之前，都属于变量tmp的"死区"。

"暂时性死区"也意味着typeof不再是一个百分之百的安全的操作

```
typeof x;//ReferenceError
let x;
```

# let 和 const 命令 {#let-const-}

## 1、let命令：

### 基本用法

es6新增了let语法命令，用来声明变量。他的用法类似于var，但是所声明的变量，只在let命令所在的代码块内有效。

```
{
    let a = 10 ;
    var b =1;
}
a //ReferneceError:a is not defined.
b //1
```

上面的代码在代码块之中，分别用let和var声明了两个变量。然后在代码块之外调用这两个变量，结果let声明的变量报错，var声明的变量返回了正确的值。这表明，let声明的变量只在它所在的代码块有效。

for循环计数器，就很适合使用let命令。

```
for(let i = 0; i < 10; i++){
    //...
}
console.log(i) ;
//ReferenceError:i is not defined
```

上面代码中，计数器i只在循环体内有效，在循环体外引用就报错。

下面的代码如果使用var，最后输出的是10。

```
var a = [] ;
for(var i = 0; i < 10; i++){
    a[i] = function(){
        console.log(i) ;
    }
}

a[6]() ;//10

//如果不用es6的解决方法是闭包
var a = [] ;
for(var i = 0; i < 10; i++){
    a[i] = (function(i){
       return function(){ console.log(i) };
    })(i) ;
}

a[6]() ;//6
```

上面的代码中，变量i是var命令声明的，在全局范围内都有效，所以全局只有一个变量i。每一次循环，变量i的值都会发生改变，而循环内被赋给数组a的函数内部的console.log\(i\)，里面的i指向的就是全局的i。也就是说，所有数组a的成员里面的i，指向的都是同一个i，导致运行时输出的是最后一轮的i的值，也就是10。

如果使用let，声明的变量仅在块级作用域内有效，最后输出的是6。

```
var a = [] ;
for(let i = 0; i < 10; i++) {
    a[i] = function() {
        console.log(i);
    } ;
}
a[6]();//6
```

上面的代码中，变量i是let声明的，当前i只在本轮循环有效，所以每次循环的i其实都是一个新的变量，所以最后输出的是6。你可能会问，如果每一轮循环的变量i都是重新声明的，那它怎么知道上一轮循环的值，从而计算出本轮循环的值？这是因为javascript引擎内部会记住上一轮循环的值，初始化本轮的变量i时，就在上一轮循环的基础上进行计算。

另外，for循环还有一个特别之处，就是设置循环变量的那部分是一个父作用域，而循环体内部是一个子作用域。

```
for(let i=0;i<3;i++){
    let i = 'abc' ;
    console.log(i) ;
}
//abc
//abc
//abc
```

上面的代码正确运行，输出了3次abc。这表明函数内部的变量i与循环体i不在同一个作用域，

有各自单独的作用域。

### 不存在变量提升

var命令会发生"变量提升"现象，即变量可以在声明之前使用，值为undefined。这种现象多多少少有些奇怪，按照一般的逻辑，变量应该在声明语句之后才可以使用。

为了纠正这一种现象，let命令改变了语法行为，它声明的变量一定要在声明后使用，否则报错。

```
// var的情况
console.log(foo) ; //输出undefined
var foo = 2;

// let的情况
console.log(bar); //报错ReferenceError
let bar = 2 ;
```

上面的代码中，变量foo用var命令声明，会发生变量提升，即脚本开始运行时，变量foo已经存在了，但是没有值，所以会输出undefined。变量bar用let命令声明，不会发生变量提升。这表示在声明它之前，变量bar是不存在的，这时如果用到它，就会抛出一个错误。

### 暂时性死区

只要块级作用域内存在let命令，它声明的变量就"绑定"（binding）这个区域，不再受外部的影响。

```
var tmp = 123;
if(true){
    tmp = 'abc';//ReferenceError
    let tmp;
}
```

上面代码中，存在全局变量tmp，但是块级作用域内let又声明了一个局部变量tmp，导致后者绑定这个块级作用域，所以在let声明变量前，对tmp赋值会报错。

ES6明确规定，如果区块中存在let和const命令，这个区块对这些命令声明的变量，从一开始就形成了封闭作用域。凡是在声明之前就使用这些变量，就会报错。

总之，在代码块内，使用let命令声明变量之前，该变量是不可用的。这在语法上，称为"暂时性死区"（temporal dead zone，简称TDZ）。

```
if(true){
    // TDZ开始
    tmp = 'abc' ;// ReferenceError
    console.log(tmp) ;// ReferenceError

    let tmp; // TDZ 结束

    console.log(tmp) //undefined

    tem = 123;
    console.log(tmp) ;123
}
```

上面代码中，在let命令声明变量tmp之前，都属于变量tmp的"死区"。

"暂时性死区"也意味着typeof不再是一个百分之百的安全的操作

```
typeof x;//ReferenceError
let x;
```

上面代码中，变量x使用let命令声明，所以在声明之前，都属于x的"死区"，只要用到该变量就会报错。因此，typeof运行时就会抛出一个ReferenceError。

作为比较，如果一个变量根本没有被声明，使用typeof反而不会报错。

```
typeof undeclared_variable // "undefined"
```

上面代码中，undeclared\_variable 是一个不存在的变量名，结果返回"undefined"。所以，在没有let之前，typeof运算符是百分之百安全的，永远不会报错。现在这一点不成立了。这样的设计是为了让大家养成良好的编程习惯，变量一定要在声明之后使用，否则就会报错。

有些"死区"比较隐蔽，不太容易发现。

```
function bar(x = y,y = 2) {
    return [x,y] ;
}
bar() ;//报错
```

上面代码中，调用bar函数之所以报错（某些实现可能不报错），是因为参数x默认值等于另一个参数y，而此时y还没有声明，属于"死区"。如果y的默认值是x,就不会报错，因为此时x已经声明了。

```
function bar(x = 2,y = x) {
    return [x,y] ;
}
bar(); //[2,2]
```

另外，下面的代码也会报错，与var的行为不同。

```
var x = x;//不报错


let x = x;//报错
//ReferenceError:x is not defined
```

上面的代码报错，也是因为暂时性死区。使用let声明变量时，只要变量在还没有声明完成前使用，就会报错。上面这行就属于这个情况，在变量x的声明语句还没有执行完成前，就去取x的值，导致报错"x未定义"。

ES6规定暂时性死区和let,const语句不出现变量提升，主要是为了减少运行时错误，防止变量声明前就使用这个变量，从而导致意料之外的行为。这样的错误在ES5是很常见的，现在有了这种规定，避免此类错误就很容易了。

总之，暂时性死区的本质就是，只要一进入当前作用域，所要使用的变量就已经存在了，但是不可获取，只能等声明变量的那一行代码出现，才可以获取和使用该变量。

### 不允许重复声明

let不允许在相同作用域内，重复声明同一个变量。

```
//报错
function func() {
    let a = 10;
    var a = 1;
}

//报错
function func() {
    let a = 10;
    let a = 1;
}
```

因此，不能在函数内部重新声明参数。

```
function func(arg){
    let arg;//报错
}

function func(arg){
    {
        let arg; //不报错
    }
}
```

## 2.块级作用域

### 为什么需要块级作用域？

ES5只有全局作用域和函数作用域，没有块级作用域，这带来很多不合理的场景。

第一种场景，内层变量可能会覆盖外层变量。

```
var tmp = new Date();

function f() {
    console.log(tmp);
    if(false) {
        var tmp = 'hello world';
    }
}

f();//undefined
```

上面代码的愿意是，if代码块的外部使用外层的tmp变量，内部使用内层的tmp变量。但是，函数f执行后，输出结果为undefined，原因在与变量提升，导致内层的tmp变量覆盖了外层的tmp变量。

第二种场景，用来计数的循环变量泄漏为全局变量。

```
var s = 'hello';

for(var i = 0;i < s.length; i++){
    console.log(s[i]) ;
}

console.log(i); // 5
```

上面代码中，变量i只能来控制循环，但是循环结束后，它并没有消失，泄漏成了全局变量。

### ES6的块级作用域

let实际上为javascript新增了块级作用域。

```
function f1(){
    let n = 5 ;
    if(true){
        let n = 10;
    }
    console.log(n); //5
}
```

上面的函数有两个代码块，都声明了变量n，运行后输出5。这表示外层代码块不受内层代码块的影响。如果两次都使用var定义变量n，最后输出的值才是10。

ES6运行块级作用域的任意嵌套。

```
{{{{{let insance = 'Hello World'}}}}};
```

上面代码使用了一个五层的块级作用域。外层作用域无法读取内层作用域的变量。

```
{{{{
    {let insance = 'Hello World'}
    console.log(insance) ;//报错
}}}}
```

内层作用域可以定义外层作用域的同名变量。

```
{{{{
    let insance = 'Hello World';
    {let insance = 'Hello World'}
}}}}
```

块级作用域的出现，实际上获得广泛应用的立即执行函数表达式（IIFE）不再必要了。

```
(functon(){
    var tmp = ...;
    ...
}()) ;

//块级作用域的写法
{
    let tmp = ...;
    ...
}
```

### 块级作用域与函数声明

函数能不能在块级作用域之中声明？这是一个相当令人混淆的问题。

ES5规定，函数只能在顶层作用域和函数作用域之中声明，不能在块级作用域声明。

```
//情况一
if(true){
    function f(){
    }
}

//情况二
try{
    function f(){}
}catch(e){
    //...
}
```

上面两种函数声明，根据ES5的规定都是非法的。

但是，浏览器没有遵守这个规定，为了兼容以前的旧代码，还是支持在块级作用域之中声明函数，因此上面两种情况实际都能运行，不会报错。

ES6引入了块级作用域，明确运行在块级作用域之中声明函数。ES6规定，块级作用域之中，函数声明语句的行为类似与let，在块级作用域之外不可引用。

```
function f(){ console.log('I am outside!');}

(function(){
    if(false){
        function f(){console.log('I am inside!');}
    }
    
    f();
}()) ;
```

上面代码在es5中运行，会得到"I am inside!"，因为在if内声明的函数f会被提升到函数头部，实际运行的代码如下。

```
//ES5 环境
function f() {console.log('I am outside!');}
(function(){
    function f(){console.log('I am inside!');}
    if(false){
    }
    f();
}()) ;
```

ES6就完全不一样了，理论上会得到"I am outside!"。因为块级作用域内声明的函数类似与let，对作用域之外没有影响。但是，如果你真的在ES6浏览器中运行一下上面的代码，是会报错的，这是为什么呢？



