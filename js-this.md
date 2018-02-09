# 上下文

上下文就是函数里面的this，js里面函数的上下文是可以动态改变的。  


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

参考：[https://segmentfault.com/a/1190000000375138](https://segmentfault.com/a/1190000000375138)

## 上下文和call，apply，bind

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



