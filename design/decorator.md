# 3.11：装饰模式

就是对已经存在的某些类进行装饰，以此来扩展一些功能



### 1：背景

```
系统使用了一个外部的通用类来做具体的操作，在整个系统中有多处调用的地方，这个时候,需要将
通用类的请求的参数打印出来,这个时候,适合用装饰模式
```

### 2： 传统做法

```java
/**
 *模拟的外部提供的第三方类
 **/
public class ExternalUtil {
    public static void requestLog(String message) {
        System.out.println("request external system to log");
    }
}

/**
 *客户端A调用
 **/
public class ClientA {
    public void doSomethingA(String message) {
        System.out.println(message); //输出发送的请求信息
        ExternalUtil.requestLog(message);
    }
}

/**
 *客户端B调用
 **/
public class ClientB {
    public void doSomethingB(String message) {
        System.out.println(message);//输出发送的请求信息
        ExternalUtil.requestLog(message);
    }
}

/**
 *使用时候的场景
 **/
public class Client {
    public static void main(String[] args) {
        ClientA clientA = new ClientA();
        ClientB clientB = new ClientB();
        clientA.doSomethingA("A");
        clientB.doSomethingB("B");
    }
}
```

```
优点: 代码简洁
缺点: 比较繁琐,每一次调用，都需要手动输出日志信息
     如果有修改日志的输出格式从平常日志到json日志的输出，需要改动很多的地方。
```



### 3. 装饰模式的做法

```java
/**
 *给第三方请求新增装饰
 **/
public class ExternalDecoratorUtil {
    public static void requestLog(String message) {
        System.out.println(message);
        ExternalUtil.requestLog(message);
    }
}

/**
 *客户端A调用
 **/
public class ClientA {
    public void doSomethingA(String message) {
        ExternalDecoratorUtil.requestLog(message);
    }
}

/**
 *客户端B调用
 **/
public class ClientB {
    public void doSomethingB(String message) {
        ExternalDecoratorUtil.requestLog(message);
    }
}
```

```
优点: 关于输出日志的部分被统一在了ExternalDecoratorUtil中，如果需要做什么变更，
整个系统只需要变更此处即可
```

