## JavaScript中的this

#### this是什么

`this`不是编写时绑定，是运行时绑定的。它依赖于函数调用的上下文条件，`this`绑定与函数声明的位置没有任何关系，而与函数被调用的方式紧密相关。  
`this`是一个完全根据**调用点**而为每次函数调用建立的绑定。

```JavaScript
    function identify() {
	return this.name.toUpperCase();
    }

    function speak() {
	    var greeting = "Hello, I'm " + identify.call( this );
	    console.log( greeting );
    }

    var me = {
	    name: "Kyle"
    };

    var you = {
	    name: "Reader"
    };

    identify.call( me ); // KYLE
    identify.call( you ); // READER

    speak.call( me ); // Hello, I'm KYLE
    speak.call( you ); // Hello, I'm READER
```

`this`机制提供了更优雅的方式来隐含地“传递”一个对象引用，导致更加干净的API和更容易的复用。

以一个例子来诠释一下：

```JavaScript
    function foo(num) {
	    console.log( "foo: " + num );

	    // 追踪 `foo` 被调用了多少次
	    this.count++;
    }

    foo.count = 0;

    var i;

    for (i=0; i<10; i++) {
	    if (i > 5) {
		    foo( i );
	    }
    }
    // foo: 6
    // foo: 7
    // foo: 8
    // foo: 9

    // `foo` 被调用了多少次？
    console.log( foo.count ); // 0 -- 这他妈怎么回事……？
```

当代码执行 `foo.count = 0` 时，它确实向函数对象 `foo` 添加了一个 `count` 属性。但是对于函数内部的 `this.count` 引用，`this` 其实 根本就不 指向那个函数对象，即便属性名称一样，但根对象也不同，因而产生了混淆。
PS:其实this这里是创建了一个全局变量`count`，而且它的当前值是`NaN`

```JavaScript
    function foo(num) {
	    console.log( "foo: " + num );

	    // 追踪 `foo` 被调用了多少次
	    // 注意：由于 `foo` 的被调用方式（见下方），`this` 现在确实是 `foo`
	    this.count++;
    }

    foo.count = 0;

    var i;

    for (i=0; i<10; i++) {
	    if (i > 5) {
		    // 使用 `call(..)`，我们可以保证 `this` 指向函数对象(`foo`)
		    foo.call( foo, i );
	    }
    }
    // foo: 6
    // foo: 7
    // foo: 8
    // foo: 9

    // `foo` 被调用了多少次？
    console.log( foo.count ); // 4
```

#### 调用点(Call-site)
    调用点：函数在代码中被调用的位置(不是被声明的位置)。  

    调用栈(Call-stack)：使我们到达当前执行位置而被调用的所有方法的堆栈。  

```JavaScript
    function baz() {
        // 调用栈是: `baz`
        // 我们的调用点是 global scope（全局作用域）

        console.log( "baz" );
        bar(); // <-- `bar` 的调用点
    }

    function bar() {
        // 调用栈是: `baz` -> `bar`
        // 我们的调用点位于 `baz`

        console.log( "bar" );
        foo(); // <-- `foo` 的 call-site
    }

    function foo() {
        // 调用栈是: `baz` -> `bar` -> `foo`
        // 我们的调用点位于 `bar`

        console.log( "foo" );
    }

    baz(); // <-- `baz` 的调用点
```

PS:你可以通过按顺序观察函数的调用链在你的大脑中建立调用栈的视图，就像我们在上面代码段中的注释那样。但是这很痛苦而且易错。另一种观察调用栈的方式是使用你的浏览器的调试工具。大多数现代的桌面浏览器都内建开发者工具，其中就包含 JS 调试器。在上面的代码段中，你可以在调试工具中为 `foo()` 函数的第一行设置一个断点，或者简单的在这第一行上插入一个 `debugger` 语句。当你运行这个网页时，调试工具将会停止在这个位置，并且向你展示一个到达这一行之前所有被调用过的函数的列表，这就是你的调用栈。所以，如果你想调查this 绑定，可以使用开发者工具取得调用栈，之后从上向下找到第二个记录，那就是你真正的调用点。

