# 3.14：门面模式

是指提供一个统一的接口去访问多个子系统的多个不同的接口,
它为子系统中的一组接口提供一个统一的高层接口

### 1: 背景

```
刚刚买好的机器,属于裸机,如果需要机器能够正常使用,需要做一些列的配置，对于个人来说，几年更换一个笔记本
麻烦就麻烦点，但是对于运维来说，是经常的事情，而且里面的细节都在变化，所以需要统一的入口
```



```java
public class SystemOperate {
    public void installSystem(){
        System.out.println("安装操作系统");
    }
}

public class UserOperate {
    public void userOperate(){
        System.out.println("用户划分");
    }
}
public class SoftwareOperate {
    public void installSoftware(){
        System.out.println("软件安装");
    }
}
```





### 2: 原始做法

```java
public class Client {

    static SystemOperate systemOperate = new SystemOperate();
    static SoftwareOperate softwareOperate = new SoftwareOperate();
    static UserOperate userOperate = new UserOperate();

    public static void main(String[] args) {

        systemOperate.installSystem();
        userOperate.userOperate();
        softwareOperate.installSoftware();

    }
}

使用的时候，多个功能，拼接在一起
坏处很明显，不同的地方，如果要使用，都需要自己排列顺序再组合一边，一旦有调整，可能会调整很多地方
```





### 3: 门面模式的做法

```java
public class OperateFacade {
    private SystemOperate systemOperate = new SystemOperate();
    private SoftwareOperate softwareOperate = new SoftwareOperate();
    private UserOperate userOperate = new UserOperate();

    public void install() {
        systemOperate.installSystem();
        userOperate.userOperate();
        softwareOperate.installSoftware();
    }
}

public class Client2 {
    public static void main(String[] args){
        OperateFacade operateFacade = new OperateFacade();
        operateFacade.install();
    }
}
```

```
优点: 客户端调用的时候逻辑非常检查，对业务屏蔽了内部的实现，一旦需要调整，只需要调整其中一个地方就可以
```

