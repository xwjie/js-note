# 一等公民-函数

毫无疑问，js里面函数是一等公民，函数能有很多乱七八糟的特效。

## 函数是一个对象，可以有自己的属性，方法。

如下面的例子，我们用在函数对象上增加了一个cache属性，用于缓存数据。

```js
  function getFn(key) {
    getFn.cache = getFn.cache ||  {}
    var value = getFn.cache[key];

    if(!value){
      value = 'get some by key:' + key;
      console.log('add cache:' + key);
      getFn.cache[key] = value;
    }

    return value;
  }

  console.log(getFn('key1'));
  console.log(getFn('key2'));
  console.log(getFn('key1'));
```

如：编写实现如下的效果。

> 实现如下语法的功能：var a = add\(2\)\(3\)\(4\); //9

首先，add函数肯定是返回一个函数，否则无法链式调用下去。第二，打印的时候显示对应的数字，所以要实现函数对象的**valueOf**函数

```js
function add(a) {
  var temp = function(b) {
    return add(a + b);
  }
  temp.valueOf = temp.toString = function() {
    return a;
  };
  return temp;
}
var ans = add(2)(3)(4);
console.log(ans); // 9
```

参考：[http://www.cnblogs.com/zichi/p/4362292.html，注意，这个贴里面的这种实现方式有问题](http://www.cnblogs.com/zichi/p/4362292.html，注意，这个贴里面的这种实现方式有问题)

```js
function add(num){
  num += ~~add;
  add.num = num;
  return add;
}
add.valueOf = add.toString = function(){return add.num};
var ans = add(3)(4)(5)(6);  // 18
alert(ans);
```

原因很简单，因为add.num相当于全局变量。下次调用值会错误。

## 上下文和call，apply，bind

上下文就是函数里面的this，js里面函数的上下文是可以动态改变的。

call和apply是调用，作用一样，只是函数接受参数的方式不太一样，**apply接受的数组，使用的较多**，尤其是不知道参数个数或者不关心参数是啥的时候。

```js
  var func = function(arg1, arg2) {};
  // 就可以通过如下方式来调用：
  func.call(this, arg1, arg2);
  func.apply(this, [arg1, arg2])


  // 定义一个函数直接调用console.log
  function log(){
    console.log.apply(console, arguments);
  };
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

Math.max 是不能接受数组格式的参数的，我们可以使用apply。如果

```js
  // 使用apply让函数支持数组格式参数
  var a = [1, 2, 3, 6, 5, 4];
  var ans = Math.max.apply(null, a);
  console.log(ans);  // 6
```



