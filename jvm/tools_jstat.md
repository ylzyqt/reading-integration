# 4.2.1：jstat

类装载、内存、垃圾收集、jit编译的信息



### 1: [命令](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/jstat.html)

```
1.1 : jps 拿到pid
1.2 : jstat -gcutil 48421
```





### 2: 运行效果

```shell
  S0     S1     E      O      M     CCS    YGC     YGCT    FGC    FGCT     GCT
  0.00  15.11  36.72  58.65  92.94  88.42    481    4.095    16    2.401    6.496
```

