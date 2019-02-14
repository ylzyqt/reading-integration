# 3.17：策略模式

就是把具体的算法实现从业务逻辑中剥离出来，成为一系列独立算法类，使得它们可以相互替换

### 1. 业务背景

```
监控系统往往会从运行的系统中,收集海量的数据,这部分数据,会有如下的处理策略:
- 大部分在统计完成之后,即会被丢弃
- 小部分的数据,为了展示完整的调用链路,需要被完整的写入磁盘
- 错误的数据,需要被完整的记录
总结下来,当前就是2种策略,一种是日志数据完整的存储,一种是存储统计报表数据
```



### 2. 传统做法

```java
public class OldStrategy {
    //存储全部
    private static final int WRITE_ALL = 0;
    //存储报表
    private static final int WRITE_REPORT = 1;

    public static void main(String[] args) {
        OldStrategy oldSampler = new OldStrategy();
        oldSampler.sampler(2);
    }

    public void sampler(int samplerType) {
        switch (samplerType) {
            case WRITE_ALL:
                System.out.println("向磁盘写入完整的数据");
                break;
            case WRITE_REPORT:
                System.out.println("向磁盘写入报表数据");
                break;
            default:
                System.out.println("未知操作类型");
        }
    }
}
```

```
优点: 代码结构简单,如上所示,根据传入的samplerType来进行操作
缺点: 
  1: 代码维护比较麻烦
  2: 具体操作的过程(向磁盘写入数据),并不如代码中那样简单,可能会变成众多代码的融合
```



### 3. 策略模式的做法

```java
/**
 * 策略接口
 **/
public interface Strategy {
    void operate();
}

/**
 * 全写入操作
 **/
public class WriteAllStrategy implements Strategy {
    @Override
    public void operate() {
        System.out.println("向磁盘写入完整的数据");
    }
}

/**
 * 写入报表操作
 **/
public class WritePartStrategy implements Strategy {
    @Override
    public void operate() {
        System.out.println("向磁盘写入报表数据");
    }
}

/**
 * 未知操作
 **/
public class WriteNoneStrategy implements Strategy {
    @Override
    public void operate() {
        System.out.println("未知操作类型");
    }
}

/**
 * 策略上下文
 **/
public class StrategyContext {
    private Strategy strategy;

    public StrategyContext(Strategy strategy) {
        this.strategy = strategy;
    }

    public Strategy getStrategy() {
        return strategy;
    }

    public void setStrategy(Strategy strategy) {
        this.strategy = strategy;
    }

    public void operate() {
        strategy.operate();
    }

}

/**
 * 客户端使用类
 * 从代码中可以看出，具体的操作完全是有客户端使用的时候决定使用何种策略
 **/
public class Client {
    public static void main(String[] args) {
        StrategyContext context = new StrategyContext(new WriteAllStrategy());
        context.operate();

        context = new StrategyContext(new WritePartStrategy());
        context.operate();

        context = new StrategyContext(new WriteNoneStrategy());
        context.operate();
    }
}
```

```
缺点: 代码稍显繁琐
优点: 接口更为清楚，扩展性更强
```

