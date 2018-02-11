# IIFE立即执行函数

**不是定义了一个函数然后立马执行。而是把函数解析成表达式。**

在一个表达式后面加上括号\(\)，该表达式会立即执行，但是在一个语句后面加上括号\(\)，是完全不一样的意思，他的只是分组操作符。

```js
function foo(){ /* code */ }(); // Uncaught SyntaxError: Unexpected token )
function foo(){ console.log('invoked'); }( 1 ); // 1，但函数并没有执行！

// 因为它完全等价于下面这个代码，一个function声明后面，又声明了一个毫无关系的表达式： 
function foo(){ console.log('invoked'); }

( 1 );
```

如：

```js
() // Uncaught SyntaxError: Unexpected token )
(1) // 1
```



因为JavaScript里括弧\(\)里面不能包含语句，所以在这一点上，解析器在解析function关键字的时候，会将相应的代码解析成function表达式，而不是function声明。引自： [http://www.cnblogs.com/TomXu/archive/2011/12/31/2289423.html](http://www.cnblogs.com/TomXu/archive/2011/12/31/2289423.html)

```js
(function () { /* code */ } ()); // 推荐使用这个
(function () { /* code */ })(); // 但是这个也是可以用的
```

还可以加在function前面加！、+、 -甚至是逗号等到都可以起到函数定义后立即执行的效果，而（）、！、+、-、=等运算符，都将函数声明转换成函数表达式，消除了javascript引擎识别函数表达式和函数声明的歧义，告诉javascript引擎这是一个函数表达式，不是函数声明，可以在后面加括号，并立即执行函数的代码。

加括号是最安全的做法，因为！、+、-等运算符还会和函数的返回值进行运算，有时造成不必要的麻烦。

引自： [http://www.jb51.net/article/50967.htm](http://www.jb51.net/article/50967.htm)

