# js的数字number

## 经典题目 0.1+0.2

首先，这不是js的问题，是计算机如何存放小数的问题。计算机里面的**只能表示分母为2的次方的小数**，如1/2=0.5，1/4=0.25，其他的在计算机里面都是除不尽的无限循环的小数。

js里面的 0.1+0.2 不是 0.3，而是  0.30000000000000004。但并不是所有小数运行都会出错，因为有 \`负负得正\` 。

```js
console.log(0.1+0.2); // 0.30000000000000004
console.log(0.1+0.1); // 0.2
console.log(0.1+0.3); // 0.4
```

参考： [https://segmentfault.com/a/1190000012175422](https://segmentfault.com/a/1190000012175422)

**大部分语言**里面，0.1+0.2都不等于0.3，而等于0.30000000000000004。  java里面，默认小数的double类型。0.1+0.2也不等于0.3。

```java
public static void main(String[] args) {
    System.out.println(0.1 + 0.2); // 0.30000000000000004
    System.out.println(0.1f + 0.2f); // 0.3
    System.out.println(0.1d + 0.2d); // 0.30000000000000004
}
```

## parseInt和parseFloat

要注意的是`parseInt('123aaa') // 123 `

```js
console.group('parseInt');
console.log(parseInt('77')); // 77
console.log(parseInt('0x77')); // 119
console.log(parseInt('077'));  // 77 , 注意：我记得之前的版本，会把他当8机制，但现在测试没有发现这种情况出现

// 十进制转换，可选（2，8，10，16）
console.log(parseInt('077', 10)); //77
console.groupEnd();



console.group('parseInt 非数字');
console.log('aaa', parseInt('aaa')); // NaN
console.log('啊啊啊', parseInt('啊啊啊')); // NaN
console.log('123aaa', parseInt('123aaa'));  // 123 , 
console.log('aaa123', parseInt('aaa123'));  // NaN , 
console.log('{}', parseInt({})); // NaN
console.groupEnd();
```

## Number

要注意的是`Number('123aaa') // NaN， 这里和parseInt不一样`

```js
console.log('77', Number('77')); // 77
console.log('0x77', Number('0x77')); // 119
console.log('077', Number('077')); // 77
console.log('aaa', Number('aaa')); // NaN
console.log('啊啊啊', Number('啊啊啊')); // NaN
console.log('123aaa', Number('123aaa'));  // NaN， 这里和parseInt不一样 
console.log('aaa123', Number('aaa123'));  // NaN 
console.log('{}', Number({})); // NaN
```

## +转换（和Number（）结果是一模一样的）

```js
console.log('77', +'77'); // 77
console.log('0x77', +'0x77'); // 119
console.log('077', +'077'); // 77
console.log('aaa', +'aaa'); // NaN
console.log('啊啊啊', +'啊啊啊'); // NaN
console.log('123aaa', +'123aaa');  // NaN， 这里和parseInt不一样 
console.log('aaa123', +'aaa123');  // NaN 
console.log('{}', +({})); // NaN
```

需要小心 +的隐形转换，如下，2个加号会出错，如果2个加号中间有空格，则不会报错，但由于进行了隐形转换，结果是错误的。

```js
console.log('"aaa"+ +"bbb"',  "aaa"+ +"bbb");  // aaaNaN
// console.log('"aaa"+ +"bbb"', "aaa"++"bbb");  // Uncaught ReferenceError: Invalid left-hand side expression in postfix operation
```

lint风格检查里面，不允许++运算符，我猜这也是一个原因吧。

## 弱类型转换

注意字符串+0和-0的不同！字符串加其他东西会把别的转换字符串。

```js
var str = '123.45';

console.log('+str',  +str);  // 123.45
console.log('-str',  -str);  // -123.45
console.log('str+0',  str+0);  // 注意：这里是字符串 '123.450'
console.log('str-0',  str-0);  // 123.45
console.log('str*1',  str*1);  // 123.45
console.log('str/1',  str/1);  // 123.45
```



