# 4.2.2：jinfo



### 1: 命令

```
1.1 jps查看 pid

1.2 jinfo -flag UseSerialGC 48421
```





### 2: 运行效果

```
案例一:
jinfo -flag UseSerialGC 48421
结果:
-XX:-UseSerialGC

案例二:
jinfo -flag UseConcMarkSweepGC 48421
结果:
-XX:+UseConcMarkSweepGC

```

