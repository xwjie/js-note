# clone属性-Object.assign

参考 Object.assign 方法。 [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/Object/assign](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)

```js
console.group('使用Object.assign是浅copy');

let obj1 = { a: 0 , b: { c: 0}};
let obj2 = Object.assign({}, obj1);
console.log(JSON.stringify(obj2)); // { a: 0, b: { c: 0}}

obj1.a = 1;
console.log(JSON.stringify(obj1)); // { a: 1, b: { c: 0}}
console.log(JSON.stringify(obj2)); // { a: 0, b: { c: 0}}

// b属性为对象，是引用关系
obj2.b.c = 3;
console.log(JSON.stringify(obj1)); // { a: 1, b: { c: 3}}
console.log(JSON.stringify(obj2)); // { a: 0, b: { c: 3}}

// Deep Clone
obj1 = { a: 0 , b: { c: 0}};
let obj3 = JSON.parse(JSON.stringify(obj1));
obj1.a = 4;
obj1.b.c = 4;
console.log(JSON.stringify(obj3)); // { a: 0, b: { c: 0}}
console.groupEnd();
```

## 注意点

可以使用 转换json字符串再转换成json对象的方式实现深copy的时候，但是`json转换的时候，很多东西会丢失`（如方法，undefined的值），所以这种方法是有问题的。

```js
console.group('使用转换为json字符串在转换回对象的时候，很多东西会丢失');
let obj11 = { a: 0 , b: { c: 0}, d: function(){}};
let obj21 = Object.assign({}, obj11);

console.log(obj21);

let obj31 = JSON.parse(JSON.stringify(obj11));

// 这里没有d属性，因为JSON.stringify会把他忽略掉
console.log(obj31);

console.groupEnd();
```

## 合并多个对象的属性

```js
var o1 = { a: 1, b: 1, c: 1 };
var o2 = { b: 2, c: 2 };
var o3 = { c: 3 };

var obj = Object.assign({}, o1, o2, o3);
console.log(obj); // { a: 1, b: 2, c: 3 }
```



