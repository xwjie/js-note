# JSON.stringify函数

`JSON.stringify()`方法是将一个JavaScript值\(对象或者数组\)转换为一个 JSON字符串，如果指定了replacer是一个函数，则可以替换值，或者如果指定了replacer是一个数组，可选的仅包括指定的属性。

参考： https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global\_Objects/JSON/stringify

## 字符串加上引号

在一些表达式里面，我们要嵌入字符串，需要把字符串加上引号，我们可以使用 '"' + str + '"'，或者 \`"${str}"\`，更加专业的方法是使用 JSON.stringify函数。

![](/assets/json.stringify.png)

## 忽略的值

* 非数组对象的属性不能保证以特定的顺序出现在序列化后的字符串中。
* 布尔值、数字、字符串的包装对象在序列化过程中会自动转换成对应的原始值。

* `undefined、`任意的函数以及 symbol 值，在序列化过程中会被忽略（出现在非数组对象的属性值中时）或者被转换成  
  `null`（出现在数组中时）。

* 所有以 symbol 为属性键的属性都会被完全忽略掉，即便`replacer`参数中强制指定包含了它们。
* 不可枚举的属性会被忽略

```js
// 下面除了null，其他都会忽略
var obj = {
  a: null, // 保留
  b: undefined, // undefined， 对象中忽略，数组中被转换成 null
  c: Number, // 函数， 对象中忽略，数组中被转换成 null
  d: function(){}, // 函数， 对象中忽略，数组中被转换成 null
  e: Symbol(""), // Symbol， 对象中忽略，数组中被转换成 null
};

console.log( JSON.stringify(obj)); // {"a": null}
console.log( JSON.stringify(obj, null, '\t' )); // 格式化输出

var array1 = [
  null, // 保留
  undefined, // 转换成 null
  Number, // 转换成 null
  function(){}, // 转换成 null
  Symbol(""), // 转换成 null
];

console.log( JSON.stringify(array1));  // [null,null,null,null,null]

// 不可枚举的属性默认会被忽略：
var obj2 = Object.create(
        null, 
        { 
            x: { value: 'x', enumerable: false }, 
            y: { value: 'y', enumerable: true } 
        }
    ); 
console.log( JSON.stringify(obj2)); // "{"y":"y"}"
```

## 格式化输出

console.log\( JSON.stringify\(obj, null, '\t' \)\);  // 格式化输出

## 覆盖对象的toJSON方法

可以实现自定义序列号之后的内容。

```js
var obj = {
  foo: 'foo',
  toJSON: function () {
    return 'bar';
  }
};
console.log(JSON.stringify(obj));      // '"bar"'
console.log(JSON.stringify({x: obj})); // '{"x":"bar"}'
```

## 使用replacer参数自定义序列化

可以过滤掉某些字段，或者改变序列号之后的输出。

```js
function replacer(key, value) {
  if (typeof value === "string") {
    return undefined;
  }
  return value;
}

var foo = {foundation: "Mozilla", model: "box", week: 45, transport: "car", month: 7};

// replacer 是个函数
console.log( JSON.stringify(foo, replacer) ); // {"week":45,"month":7}

// replacer 是个数组，只序列号指定字段
console.log( JSON.stringify(foo, ['week', 'month']) ); // {"week":45,"month":7}
```

## 测试结果：

![](/assets/json.stringify-all.png)

