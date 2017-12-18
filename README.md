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



