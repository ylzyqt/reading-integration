# 4.2.1：jps

jvm process status 类似于任务管理器



### 1: 命令:

```
jps -l

-m 显示接收运行时参数
-v 显示接收的vm的参数

可以全部加一起用:  jps -mlv
```



### 2: 运行结果

```shell
49712 sun.tools.jps.Jps
48436 org.jetbrains.idea.maven.server.RemoteMavenServer
48421
49710 org.jetbrains.jps.cmdline.Launcher
49711 com.you.study.tools.JpsTest
```

