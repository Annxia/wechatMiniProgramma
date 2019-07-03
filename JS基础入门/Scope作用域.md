## JavaScript的作用域

#### 1. 何为作用域
> 几乎所有编程语言最基本的功能之一,就是能够储存变量当中的值,并且能在之后对这个 值进行访问或修改。事实上,正是这种储存和访问变量的值的能力将状态带给了程序。 

作用域是你的代码在运行时,某些特定部分中的变量,函数和对象的可访问性。换句话说，作用域决定了变量与函数的可访问范围，即作用域控制着变量与函数的可见性和生命周期。

#### 2. 全局作用域

拥有全局作用域的对象可以在代码的任何地方访问到，在js中一般有以下几种情形拥有全局作用域:  
1. 最外层的函数以及最外层变量  

```JavaScript
    var globleVariable = 'global';      //最外层变量
    function globalFunc(){              //最外层函数
        var childVariable = 'global_child'; //函数的内变量
        function childFunc(){               //内层函数
            console.log(childVariable);
        }
        console.log(globleVariable);
    }
    console.log(globleVariable);    //global
    globalFunc();                   //global
    console.log();                  //childVariable is not defined
    console.log(childFunc);         //childFunc is not defined
```  
 
 2. 未定义直接赋值的变量(由于变量提升使之成为全局变量)  

 ```JavaScript
    function func1(){
        special = 'special_variable';
        var normal = 'normal_variable';
    }
    func1();
    console.log(special);    //special_variable
    console.log(normal)     // normal is not defined
 ```
虽然我们可以在全局作用域中声明函数及变量，使之成为全局变量，但是不建议这么做，因为这可能会和其他变量名冲突，一方面如果我们再使用`const`或者`let`声明变量，当命名发生冲突时会报错。  

#### 3. 局部作用域
和全局作用域相反，局部作用域一般只能在固定代码片段中使用，最常见的就是**函数作用域**

##### 3.1 函数作用域
> 定义在函数中的变量就在函数作用域中。并且函数在每次调用时都有一个不同的作用域。这意味着同名变量可以用在不同的函数中。因为这些变量绑定在不同的函数中，拥有不同作用域，彼此之间不能访问。  

```JavaScript
    function test(){
        var num = 9;
        // 内部可以访问
        console.log("test中："+num);
    }
    //test外部不能访问
    console.log("test外部:"+num);
```

下面这个例子：js中的`var`变量只有全局作用域和函数作用域两种。所以这个i在for循环中被声明，作用域是全局的。
```JavaScript
    var name = '程序员成长指北';
    for(var i=0; i<5; i++){
        console.log(i)
    }
    console.log('{}外部:'+i);
    // 0 1 2 3 4  {}外部:5

```  

##### 3.2 变量提升

```JavaScript
    var tmp = new Date();
    function f() {
        console.log(tmp);
        if(false) {
         var tmp='hello';
    }
}

```
这道题的正确答案居然是`undefined`。这是由于变量提升造成的，等同于以下的代码：

```JavaScript
    var tmp = new Date();
    function f() {
        var tmp;
        console.log(tmp);
        if(false) {
            tmp='hello';
        }
    }
    f();

```  
console在输出的时候，tmp变量仅仅声明了但未定义。所以输出undefined。虽然能够输出，但是并不推荐这种写法推荐的做法是在申明变量的时候，将所用的变量都写在作用域（全局作用域或函数作用域）的最顶上，这样代码看起来就会更清晰，更容易看出来哪个变量是来自函数作用域的，哪个又是来自作用域链


##### 3.3 块级作用域

>  ES6新增了`let`和`const`命令，可以用来创建块级作用域变量，使用`let`命令声明的变量只在`let`命令所在 **代码块**内有效。  

* 变量不会提升到代码块顶部，且不允许从外部访问块级作用域内部变量

```JavaScript
    console.log(bar);       //抛出`ReferenceErro`异常: 某变量 `is not defined`
    let bar=2;
    for (let i =0; i<10;i++){
        console.log(i)
    }
    console.log(i);         //抛出`ReferenceErro`异常: 某变量 `is not defined`

```

上面例子中的`i`就是只在for循环内部使用，不会去污染整个作用域。

* 不允许反复声明
    ES6中的`let`和`const`不允许重复声明，与var不同
```JavaScript
    // var
    function test(){
        var name = 'koloa';
        var name = '程序员成长指北';
        console.log(name); // 程序员成长指北
    }

    // let || const
    function test2(){
        var name ='koloa';
        let name= '程序员成长指北'; 
        // Uncaught SyntaxError: Identifier 'count' has already been declared
    }

```

##### LHS 和 RHS
引擎的查找方式有两种：LHS(Left-Hand Side)和RHS(Right-Hand Side)。  
如果这句话是赋值能力的语句，会执行LHS。  
如果这句话是取值能力的语句，会执行RHS。  
```JavaScript
    function foo(a){
        var b = a;
        return a + b;
    }

    var c = foo(2);
```
上面这段代码中有那些LHS和哪些RHS呢？  
`var c = foo(2);` 中对foo进行RHS  
`var c = foo(2);` 中对c进行LHS  
`function foo(a)` 中对a(形参)进行赋值LHS  
`var b = a;` 函数体要使用a，所以进行取值RHS  
`var b = a;` 中对b进行赋值操作LHS，把a赋值给b
`return a+b;` 对a进行RHS  
`return a+b;` 对b进行RHS        






