# 上下文

参考： [http://www.cnblogs.com/TomXu/archive/2012/01/12/2308594.html](http://www.cnblogs.com/TomXu/archive/2012/01/12/2308594.html)

上下文就是函数里面的this，this 是进入上下文时确定，并且在上下文运行期间永久不变。在一个函数代码中，这个值在每一次完全不同。\(this不是变量，不能赋值\)。

首先，在通常的函数调用中，this 是由激活上下文代码的调用者来提供的，即调用函数的父上下文 \(parent context\)。this 取决于调用函数的方式。

参考： [http://www.cnblogs.com/TomXu/archive/2012/01/17/2310479.html](http://www.cnblogs.com/TomXu/archive/2012/01/17/2310479.html)

```js
var User = {
  count: 1,

  getCount: function() {
    return this.count;
  }
};

console.log(User.getCount()); //1

var func = User.getCount;
console.log(func()); //undifined
```

第一次调用，在user对象里面调用，this就是user，第二次调用func是在window下，所以是undifined。

帖子里还有手工实现bind的代码。\(可以看出，bind返回了一个新函数，里面的self就是函数\)

```js
// 手工实现bind函数
Function.prototype.bind = Function.prototype.bind || function(context){
  var self = this;

  return function(){
    return self.apply(context, arguments);
  };
}
```

参考：[https://segmentfault.com/a/1190000000375138](https://segmentfault.com/a/1190000000375138)

## 

## 我的理解

**严格模式下，不是a.b的调用从时候，this是undefined，非严格模式下，默认会是window。**

```js
a.b(); // a 就是this

var func = User.getCount;
console.log(func()); // this = window

// 但要注意下面这种情况
// 只要方法不是已 a.b 调用的，那么a就是undefined，就是说this是undefined，默认就是指向window
// 如下代码的bar和bar2，他们都不是a.b的这种调用形式，那么this就是window。
function foo() {
  console.log('this1', this);

  function bar() {
    console.log('this2', this); // 永远是 global
  }

  var bar2 = function(){
    console.log('this3', this);  // 永远是 global
  }

  bar(); 
  bar2();
}

foo(); // this1，this2 ，this3 =window
foo.call({}); // this1={}, this2，this3=window

// 还有直接执行的函数，this都是undefined，默认都是window
(function(){console.log(this);}()); // this总是window
```

发现理解有偏差，严格模式下

```js
'use strict'

var func = User.getCount;

console.log(func()); // this = undefined
console.log(window.func()); // this = window

// but 这样定义的函数，2种情况都是window，不知道为什么！
function f1(){console.log(this);}

console.log(f1()); // this = window
console.log(window.f1()); // this = window
```

## 上下文和call，apply，bind

call和apply是 `调用`，作用一样，只是函数接受参数的方式不太一样，**apply接受的数组，使用的较多**，尤其是不知道参数个数或者不关心参数是啥的时候。

```js
  var func = function(arg1, arg2) {};
  // 就可以通过如下方式来调用：
  func.call(this, arg1, arg2);
  func.apply(this, [arg1, arg2])


  // 定义一个函数直接调用console.log
  function log(){
    console.log.apply(console, arguments);
  };

  // 更加简单的是 log = console.log ，但这个时候log是个变量，注意变量提升问题。
```

**bind不是调用，而是返回一个新的函数**，bind的参数就是上下文。常见使用场景如下：

```js
var foo = {
    bar : 1,
    eventBind: function(){
        $('.someClass').on('click',function(event) {
            console.log(this.bar);      //1
        }.bind(this));
    }
}
```

就不需要用 that = this 先保存上下文了。参考：[http://www.cnblogs.com/tylerdonet/p/4864116.html](http://www.cnblogs.com/tylerdonet/p/4864116.html)

另外 apply 会把输入参数转成数组形式，所以可以实现下面效果。

> 找出数字数组中最大的元素（使用Math.max函数）

Math.max 是不能接受数组格式的参数的，我们可以使用apply。如下

```js
  // 使用apply让函数支持数组格式参数
  var a = [1, 2, 3, 6, 5, 4];
  var ans = Math.max.apply(null, a);
  console.log(ans);  // 6
```

## 使用call或者apply保护默认方法

语言默认的方法可以被改写，这个时候我们可以这样来调用：

```js
(function(){
  var obj = {
      hasOwnProperty: function(){
        return false;
      },
      some: 'some prototype'
  };

  // hasOwnProperty 被改写了
  console.log(obj.hasOwnProperty('some')); // false

  // 使用call来调用object上原来的方法
  console.log(Object.hasOwnProperty.call(obj, 'some')); // true
  console.log(({}).hasOwnProperty.call(obj, 'some')); // true

  // 能正确找到
  for(var key in obj){
    console.log(key, obj[key]);
  }

  console.log('keys',Object.keys(obj)); // ["hasOwnProperty", "some"]
  console.log('keys',Object.getOwnPropertyNames(obj)); // ["hasOwnProperty", "some"]

}())
```



