# 相等判断

这是js里面坑最多的地方之一，会出现很多奇葩的现象。

如`if(2)`是true，`if(2==true)`就是false，`if(1==true)`又是true。还有`if([]==[])`是false，`if([]==![])`是true等等。

## 结论

1. js 中 0、-0、null、""、false、undefined 和 NaN 为 false，**其他都为true**
2. 二边都是对象比较的时候，只要不是同一个对象，都不相等
3. 二边类型不一致的时候，最终会**转换为数字进行对比**，true=1，false=0



