# js的小括号

## 

## 执行多个表达式，并返回最后一个表达式的值

```js
(1,2+3,4) //4
```

# 1和\(1\)

1是基本类型，\(1\)是对象。

1.toString\(\)报错，\(1\).toString\(\) 正确执行。

个人可以理解为java的装箱操作，不知道是否正确？ 如：

```js
// 直接1.toString 报错

function a(){return 1};
a().toString() // "1"

(1).toString() // "1"
```

## 实现如下语法的功能：var a = \(5\).plus\(3\).minus\(6\); //2

很明显应该在Number原型上加方法。

```js
Number.prototype.plus = function(a) {
  return this + a;
};

Number.prototype.minus = function(a) {
  return this - a;
};

var a = (5).plus(3).minus(6);
console.log(a); // 2
```



