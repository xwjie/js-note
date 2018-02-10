# js的小数点

注意1个点和2个点的区别。

```js
1.toString() //Uncaught SyntaxError: Invalid or unexpected token
1..toString() //"1"
1['toString']() //"1"
```

![](/assets/js-dot.png)

