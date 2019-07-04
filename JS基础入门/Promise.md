## Promise

####  什么是Promise?

Promise
是异步编程的一种解决方案，比传统的解决方案——回调函数和事件——更加合理和强大。  

本质上，Promise是一个绑定了回调的对象，而不是将回调传进函数内部。  

**Promise对象的特点**
1. 对象的状态不受外界改变。Promise对象代表一个异步操作，有三种状态：`pending`(进行中)，`fulfilled`(已完成)和`rejected`(已失败)。只有异步操作的结果，可以决定当前是哪种状态，任何其他操作都无法改变这个状态。  
2. 一旦状态改变，就不会再变，任何时候都可以得到这个结果。Promise对象的状态改变，只有两种可能：从`pending`变为`fulfilled`和从`pending`变为`rejected`。只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果，这时就称为 resolved（已定型）。如果改变已经发生了，你再对Promise对象添加回调函数，也会立即得到这个结果。这与事件（Event）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到结果的。  


#### 基本用法
ES6规定，`Promise`对象是一个构造函数，用来生成Promises实例  

```JavaScript
    const promise = new Promise(function(resolve, reject) {
    // ... some code

        if (/* 异步操作成功 */){
            resolve(value);
        } else {
            reject(error);
        }
    });
```

#### Promise.prototype.then()

Promise 实例具有`then`方法，也就是说，`then`方法是定义在原型对象`Promise.prototype`上的。它的作用是为 Promise 实例添加状态改变时的回调函数。`then`方法的第一个参数是`resolved`状态的回调函数，第二个参数（可选）是`rejected`状态的回调函数。  

`then`方法返回的是一个新的Promise实例（注意，不是原来那个Promise实例）。因此可以采用链式写法，即`then`方法后面再调用另一个then方法。

```JavaScript
    getJSON("/posts.json").then(function(json) {
      return json.post;
    }).then(function(post) {
      // ...
    });

```
上面的代码使用`then`方法，依次指定了两个回调函数。第一个回调函数完成以后，会将返回结果作为参数，传入第二个回调函数。  


#### Promise.prototype.catch()

`Promise.prototype.catch`方法是`.then(null, rejection)`或`.then(undefined, rejection)`的别名，用于指定发生错误时的回调函数。

```JavaScript
    getJSON('/posts.json').then(function(posts) {
      // ...
    }).catch(function(error) {
      // 处理 getJSON 和 前一个回调函数运行时发生的错误
      console.log('发生错误！', error);
    });

```

上面代码中，`getJSON`方法返回一个 Promise 对象，如果该对象状态变为`resolved`，则会调用then方法指定的回调函数；如果异步操作抛出错误，状态就会变为`rejected`，就会调用`catch`方法指定的回调函数，处理这个错误。另外，`then`方法指定的回调函数，如果运行中抛出错误，也会被`catch`方法捕获。

