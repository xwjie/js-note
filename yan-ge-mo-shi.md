# strict严格模式

参考： https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Strict\_mode

在非严格模式下，很多错误语句不会报错，但运行的结果也不会是我们预期的结果。如

```js
(function(){
  var s = 'abc';
  s.a = 1;
  console.log(Object.isExtensible(s)); // false
  console.log(Object.isFrozen(s)); // true
  console.log(Object.isSealed(s));  // true
  console.log(s.a); // undefined;
}());
```

这个时候我们需要使用严格模式，错误的语句执行的时候就会报错！

```js
(function(){
  'use strict'
  var s = 'abc';
  console.log(Object.isExtensible(s)); // false
  console.log(Object.isFrozen(s)); // true
  console.log(Object.isSealed(s));  // true
  s.a = 1;
  console.log(s.a); // Uncaught TypeError: Cannot create property 'a' on string 'abc'
}());
```



