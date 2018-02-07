# javascript标签运行机制

## 结论：

1. 一个一个块顺序执行
2. 上一个块的报错不影响下一个块的执行
3. 能使用前面的代码块的数据，但不能使用后面的代码

> 这里说的块为script标签

## 测试

```html
<script>
console.group("block1");
var a = 1;
console.log('block1-1', b);  //b由于声明提前，这里是undifined
console.log('block1-2', a2); // a2为下一个块的定义，无法访问，报 ReferenceError
var b = 2; //上一句话已经报错，这里不会执行
</script>

<script>
console.groupEnd(); // groupEnd放在前面是保证能执行
console.group("block2");
var a2 = 'a2';
console.log('block2-1', a);
console.log('block2-2', b); //b为上一个块的定义，由于声明提前，这里是undifined
</script>

<script>
console.groupEnd();
console.group("block3");
console.log('block3-1', f4); // a2为下一个块的定义，无法访问，报 ReferenceError
console.log('block3-2', f3); // 上一句话已经报错，这句话不会执行
function f3(){}
</script>

<script>
console.groupEnd();
console.group("block4");
console.log('block4-1', f4); // 声明式函数会提前，由于声明提前，这里结果正确
console.log('block4-2', f3); //f3为上一个块的定义，由于声明提前，这里结果正确
function f4(){}
console.groupEnd();
</script>
```

## 结果

![](/assets/js-tag.png)



