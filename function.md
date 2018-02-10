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

第二次传入 key1 的时候，由于已经缓存，就可以直接返回之前的缓存数据。



## 实现如下语法的功能：var a = add\(2\)\(3\)\(4\); //9



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

参考：[http://www.cnblogs.com/zichi/p/4362292.html](http://www.cnblogs.com/zichi/p/4362292.html)

注意，这个贴里面的这种实现方式有问题

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

原因很简单，因为 `add.num`相当于全局变量。下次调用值会错误。

## 赋值型函数可以有名字，但名字不是变量名，不会提升

![](/assets/funciton-1.png)

这里的A可以做为构造函数，就是可以 new A\(\)。

## 箭头函数无法做为构造函数

## ![](/assets/funciton-2.png) 

## 



