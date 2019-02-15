# 3.16：不变模式

一个对象的状态,在对象被创建之后,就不再发生变化

弱不变: 对象不变,属性可以变

强不变: 对象不变,属性也不可以变



### 1. 背景

```
代码中有的类,自动生成出来之后,就不希望发生变化,比如系统启动时，拿到的启动参数相关信息
```



### 2. 不变模式的方案

```java
public final class SystemProperties {
    //环境
    private final String environment;
    //主机名称
    private final String hostname;
    
    public SystemProperties(String environment, String hostname) {
        this.environment = environment;
        this.hostname = hostname;
    }

    public String getEnvironment() {
        return environment;
    }

    public String getHostname() {
        return hostname;
    }
}
```

