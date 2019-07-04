## JavaScript小Tips

#### `==` 和 `===`
`==`在允许强制转换的条件下比较值的等价性;`===`是在不允许强制转换的条件下检查值的等价性，因此`===`
常被称为“严格等价”。  

```JavaScript
    var a = "42";
    var b = 42;

    a == b;     //true
    a === b;    //false
```  
如果你在比较两个非基本类型值，比如`object`(包括`function`和`array`),需要特别注意，因为这些值实际上是通过引用持有的。

```JavaScript
    var a = [1,2,3];
    var b = [1,2,3];
    var c = "1,2,3";

    a == c;         //true
    b == c;         //true
    a == b;         //false
```  
上面这个例子真的有点让人范懵，array在js中不是基本类型，即Object类型，这时`==`判断的是两个引用是否相等,那引用指向的是内存地址，`a==b`当然是`false`咯;  
而在a和c的比较中,`==`先将a和c进行强制类型转换了，具体转换成什么了就得去看底层实现了。

#### typeof 和 instanceof

##### typeof
typeof方法返回一个字符串，来表示数据的类型
|数据类型|type|
|:-----:|:----:|
|Undefined|undefined|
|Null|object|
|布尔值|boolean|
|数值|number|
|字符串|string|
|Symbol|symbol|
|函数对象|function|
|任何其他对象|object|  

typeof来判断数据类型其实并不准确。比如数组、正则、日期、对象的typeof返回值都是object，这就会造成一些误差。  
所以在typeof判断类型的基础上，我们还需要利用Object.prototype.toString方法来进一步判断数据类型。  
|数据|toString|typeof|
|:-----:|:-----:|:-----:|
|"foo"|String|string|
|new String("foo")|String|object|
|new Number(1.2)|Number|object|
|true|Boolean|boolean|
|new Boolea(true)|Boolean|object|
|new Date()|Date|object|
|new Error()|Error|object|
|new Array(1,2,3)|Array|object|
|/abc/g|RegEcp|object|
|new RegExp("neow")|RegExp|object|  

可以看到利用toString方法可以正确区分出Array、Error、RegExp、Date等类型。  

**真题测验**
```JavaScript
    var y = 1,x = y = typeofx;
    x;      //"undefined"
```
表达式是从右往左的，x由于 *变量提升*,类型不是null，而是undefined,所以 x = y = "undefined"

```JavaScript
    (function f(f){
        return typeof f();      //"number"
    })(function(){return 1;});
```
传入的参数为f，也就是 function(){return 1;}这个函数。通过 f() 执行后，得到结果 1。  

```JavaScript
    var foo = {
        bar: function(){ return this.baz;},
        baz: 1
    };
    (function(){
        return typeof argumments[0]();      //"undefined"
    })(foo.bar);
```
这一题考察的是this的指向。**this永远指向函数执行时的上下文，而不是定义时的(ES6的箭头函数不算)**。当arguments执行时，this已经指向了window对象。所以是"undefined"。  

```JavaScript
   var foo = {
       bar: function(){ return this.baz; },
       baz: 1
   } 
   typeof (f = foo.bar)();  //"undefined"
```
同上。  

```JavaScript
    var f = (function f(){ return "1";},function g(){return 2;})();
    typeof f;       //"number"
```
JavaScript的**分组选择符**，会以最后一个为准。也就是 2。

```JavaScript
   var  x = 1;
   if (function f(){}) {
       x += typeof f;
   } 
   x;       //"1undefined"
```
这是一个javascript语言规范上的问题，在条件判断中加入函数声明。这个声明语句本身没有错，也会返回true，但是javascript引擎在搜索的时候却找不到该函数。所以结果为“1undefined”

```JavaScript
    (function(foo){
        return typeof foo.bar;
    })({foo: { bar:1 }});       //"undefined"
```
这题其实是一个考察心细程度的题目。形参的foo指向的是{ foo: { bar: 1 } }这个整体。相信这么说就明白了。


##### instanceof
instanceof运算符可以用来判断某个构造函数的prototype属性是否存在于另外一个要检测对象的原型链上。  

instanceof 左操作数是一个类，右操作数是标识对象的类。如果左侧的对象是右侧类的实例，则返回true.而js中对象的类是通过初始化它们的构造函数来定义的。即instanceof的右操作数应当是一个函数。所有的对象都是object的实例。如果左操作数不是对象，则返回false,如果右操作数不是函数，则抛出typeError。  

instanceof 操作符用来比较两个操作数的构造函数。只有在比较自定义的对象时才有意义。 如果用来比较内置类型，将会和 typeof 操作符 一样用处不大。

``` JavaScript
    function f(){ return f; }
    document.write(new f() instanceof f);//false,因为构造函数的原型被覆盖了
    function g(){}
    document.write(new g() instanceof g);//true
```