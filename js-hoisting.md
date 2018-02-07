# 声明提升（hosting）

## 结论：

1. **当前块**声明会提升到**当前作用域**最前面
2. 函数提升的时候已经定义好，多次定义会**覆盖**之前的定义
3. 没有加var的变量，相当于定义在最外层，但并**不会**提升到最外层

## 测试

```
<script>
console.group("block1-下一块的变量不会提升到本块");
console.log("block1-1", a);//undifined
var a = 1;
console.log("block1-2", b); //变量b为下一块的变量，这里访问不了，报错ReferenceError
</script>

<script>
var b =2;
console.groupEnd();
</script>

<script>
console.group("block2-变量提升到他的作用域，不会到最外层");
f2(); //block2-2 undefined
console.log("block2-1", a2); //ReferenceError: a2 is not defined

function f2(){
  console.log('block2-2', a2); //a2会提升，但只会到当前作用域，就是函数作用域
  var a2 = 'a2'; //
}
</script>

<script>
console.groupEnd();
console.group("block3-函数提升的时候已经定义好，多次定义会覆盖之前的定义");
f3(); //block3-222

function f3(){
  console.log('block3-111');
}

f3(); // block3-222
function f3(){
  console.log('block3-222');
}
</script>

<script>
console.groupEnd();
console.group("block4-没有加var的变量，相当于定义在最外层，并不会提升到最外层");
console.log("block4-1", b4); //ReferenceError

// f4函数并没有调用
function f4(){
  b4 = 'b4'; //b2没有加var，相当于定义在最外层
}
</script>
<script>
console.groupEnd();
</script>
```

## 结果

![](/assets/js-hosting.png)

