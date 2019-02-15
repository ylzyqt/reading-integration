# 3.21：责任链模式

使多个对象都有机会处理请求，从而避免请求的发送者和接受者之间的耦合关系，
将这个对象连成一条链，并沿着这条链传递该请求



### 1: 背景

```
监控报警系统中,往往报警的条件一旦确认之后,诸多的的报警项,都需要根据该条件进行报警操作
```



```java
报警条件:
public class ChainCondition {

    //是否超时
    boolean overTime;

    //是否错误率超高
    boolean errorPercentUnNormal;

    //是否流量异常下跌
    boolean networkTrafficAbnormallyFalling;

    public boolean isOverTime() {
        return overTime;
    }

    public void setOverTime(boolean overTime) {
        this.overTime = overTime;
    }

    public boolean isErrorPercentUnNormal() {
        return errorPercentUnNormal;
    }

    public void setErrorPercentUnNormal(boolean errorPercentUnNormal) {
        this.errorPercentUnNormal = errorPercentUnNormal;
    }

    public boolean isNetworkTrafficAbnormallyFalling() {
        return networkTrafficAbnormallyFalling;
    }

    public void setNetworkTrafficAbnormallyFalling(boolean networkTrafficAbnormallyFalling) {
        this.networkTrafficAbnormallyFalling = networkTrafficAbnormallyFalling;
    }
}
```





### 2: 原始方式

```java
public class OldWay {

    public static void main(String[] args) {

        ChainCondition chainCondition = new ChainCondition();

        chainCondition.setOverTime(false);
        chainCondition.setErrorPercentUnNormal(true);
        chainCondition.setNetworkTrafficAbnormallyFalling(true);

        if (chainCondition.isOverTime()) {
            System.out.println("超时报警");
        }

        if (chainCondition.isErrorPercentUnNormal()) {
            System.out.println("错误报警");
        }

        if (chainCondition.isNetworkTrafficAbnormallyFalling()) {
            System.out.println("流量异常下跌报警");
        }

    }
}
```

```
优势: 代码量小
缺点: 众多的if语句拼接成代码快，不利于代码分层和后续扩展
```





### 3 : 责任链的方式

```java
/**
 * 责任链
 */
public abstract class AlarmChain<T> {
    
    abstract boolean needExecute(T t);
    
    void process(T t) {
        if (needExecute(t)) {
            execute();
        }
    }
    
    abstract void execute();
}

/**
 * 超时的实现类
 */
public class AlarmOverTimeChain extends AlarmChain<ChainCondition> {
    @Override
    public boolean needExecute(ChainCondition chainCondition) {
        return chainCondition.isOverTime();
    }

    @Override
    public void execute() {
        System.out.println("超时报警");
    }
}

/**
 * 错误报警的实现类
 */
public class ErrorPercentUnNormalChain extends AlarmChain<ChainCondition> {
    @Override
    public boolean needExecute(ChainCondition chainCondition) {
        return chainCondition.isErrorPercentUnNormal();
    }

    @Override
    public void execute() {
        System.out.println("错误报警");
    }
}

/**
 * 流量异常下跌的实现类
 */
public class NetworkTrafficAbnormallyFallingChain extends AlarmChain<ChainCondition> {
    @Override
    public boolean needExecute(ChainCondition chainCondition) {
        return chainCondition.isNetworkTrafficAbnormallyFalling();
    }

    @Override
    public void execute() {
        System.out.println("流量异常下跌报警");
    }
}

/**
 * 责任链的建造者
 */
public class AlarmChainBuilder<T> {

    private List<AlarmChain<T>> chains;

    private AlarmChainBuilder(ChainBuilder chainBuilder) {
        this.chains = chainBuilder.chains;
    }

    public static ChainBuilder newBuilder() {
        return new ChainBuilder();
    }


    static class ChainBuilder<T> {
        private List<AlarmChain<T>> chains = new ArrayList<>();


        public ChainBuilder add(AlarmChain<T> chain) {
            chains.add(chain);
            return this;
        }

        public AlarmChainBuilder build() {
            return new AlarmChainBuilder(this);
        }
    }

    public void execute(T t) {
        chains.forEach(x -> x.process(t));

    }
}

/**
 * 客户端调用的方式
 */
public class Client {

    public static void main(String[] args) {
        ChainCondition chainCondition = new ChainCondition();

        chainCondition.setOverTime(false);
        chainCondition.setErrorPercentUnNormal(true);
        chainCondition.setNetworkTrafficAbnormallyFalling(true);


        AlarmChainBuilder.newBuilder()
                .add(new AlarmOverTimeChain())
                .add(new ErrorPercentUnNormalChain())
                .add(new NetworkTrafficAbnormallyFallingChain())
                .build().execute(chainCondition);


    }
}
```

```
优点: 代码也比较简单，但是分层清晰，后续扩展度高
```





### 4: 责任链模式与状态模式的区别

```
都是在整个流程里面,实现多个功能项目,区别在于:
责任链: 每个责任链的环节都是独立的,不存在上下文关系,可以随时调整责任链中的顺序
状态模式: 各个环节在编码的时候就已经固定了,存在先后顺序关系
```

