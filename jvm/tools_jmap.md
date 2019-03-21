# 4.2.4：jmap

### 1: [命令](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/jmap.html)

```
1: jps 查看pid
2: jmap -dump:format=b,file=/文件位置/a.hprof 50263
```

### 2: 执行效果

```
Dumping heap to /文件位置/a.hprof ...
Heap dump file created

使用MAT工具即可进行具体的数据的分析
```