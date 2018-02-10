# prototype和constructor

原型，原型链是绕不开的话题。

参考： [http://www.ruanyifeng.com/blog/2010/05/object-oriented\_javascript\_inheritance.html](http://www.ruanyifeng.com/blog/2010/05/object-oriented_javascript_inheritance.html)

## 注意点

方法：有原型，有 constructor ，值为 Function

实例：没有原型，只有\_\_proto\_\_属性，有 constructor，值为构造方法。

## **isPrototypeOf**

The**`isPrototypeOf()`**method checks if an object exists in another object's prototype chain.

```js
function Foo() {}
function Bar() {}
function Baz() {}

Bar.prototype = Object.create(Foo.prototype);
Baz.prototype = Object.create(Bar.prototype);

var baz = new Baz();

console.log(Baz.prototype.isPrototypeOf(baz)); // true
console.log(Bar.prototype.isPrototypeOf(baz)); // true
console.log(Foo.prototype.isPrototypeOf(baz)); // true
console.log(Object.prototype.isPrototypeOf(baz)); // true
```

## **hasOwnProperty**

判断某一个属性到底是本地属性，还是继承自`prototype `对象的属性。

  


