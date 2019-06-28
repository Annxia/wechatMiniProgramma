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
这道题的正确答案居然是`undefined`。

##### 3.3 块级作用域

>  ES6新增了`let`和`const`命令，可以用来创建块级作用域变量，使用`let`命令声明的变量只在`let`命令所在 **代码块**内有效。  





