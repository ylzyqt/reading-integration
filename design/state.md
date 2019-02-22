# 3.24：状态模式

允许一个对象在其内部状态改变时改变它的行为。对象看起来似乎修改了它的类

特点: 有多个行为,根据状态来进行判断

### 1: 业务背景

```
涉及到用户的项目里面，往往都有个环节,发送短信, 假设公司共签署了3个供应商的短信通道，
分别为A,B,C 通道，根据对方的稳定性，排序从高到低为 A->B-C ,所以具备以下特性:
1: A->B-C 的顺序是固定的，这是按照稳定性排序得出的结论
2: 同一条短信，只会在A,B,C 中有一个通道来发送，不会出现多个发送的场景
3: 这里有个健康度，根据发送的成功率、发送的耗时等因素得出的，命名为状态因子
执行的结果就是，优先由A发送，加入某天A发生了故障，那么系统得自动切换到B,如果B因为压力的原因，
那么自动切换到C 由此起到一个分流的作用
```



### 2: 传统做法

```java
public class OldSender {
    public static final int CHANNEL_A = 0;
    public static final int CHANNEL_B = 1;
    public static final int CHANNEL_C = 2;

    public static void main(String[] args) {
        OldSender oldSender = new OldSender();
        oldSender.send(CHANNEL_A);
    }

    public void send(int sendType) {
        switch (sendType) {
            case CHANNEL_A:
                System.out.println("发送A通道消息");
                break;
            case CHANNEL_B:
                System.out.println("发送B通道消息");
                break;
            case CHANNEL_C:
                System.out.println("发送C通道消息");
                break;
            default:
                System.out.println("未知发送渠道");
        }

    }
}
```

```
优点: 代码写的简单
缺点: 难以维护,众多的常量定义
```



### 3: 状态模式的做法

```java
/**
 * 获取健康状态
 */
public class SmsChannelHealthyUtils {
    private static Random random = new Random();
    public static int getHealthyState() {
        return random.nextInt(100);
    }
}

public abstract class SmsChannel {

    protected SmsContext smsContext;

    public SmsChannel(SmsContext smsContext) {
        this.smsContext = smsContext;
    }

    abstract void send();
}

public class SmsChannelA extends SmsChannel {

    public SmsChannelA(SmsContext smsContext) {
        super(smsContext);
    }

    @Override
    void send() {
        if (SmsChannelHealthyUtils.getHealthyState() < 60) {
            System.out.println("通过A渠道发送");
        } else {
            smsContext.setSmsChannel(smsContext.getSmsChannelB());
            smsContext.send();
        }
    }
}


public class SmsChannelB extends SmsChannel{

    public SmsChannelB(SmsContext smsContext) {
        super(smsContext);
    }

    @Override
    void send() {
        if(SmsChannelHealthyUtils.getHealthyState() < 90){
            System.out.println("通过B渠道发送");
        }else{
            smsContext.setSmsChannel(smsContext.getSmsChannelC());
            smsContext.send();
        }
    }
}

public class SmsChannelC extends SmsChannel {

    public SmsChannelC(SmsContext smsContext) {
        super(smsContext);
    }

    @Override
    void send() {
        System.out.println("通过缺省C渠道发送");
    }
}


public class SmsContext {
    private SmsChannel smsChannel;

    private SmsChannel smsChannelA = new SmsChannelA(this);
    private SmsChannel smsChannelB = new SmsChannelB(this);
    private SmsChannel smsChannelC = new SmsChannelC(this);

    public SmsContext() {
        this.smsChannel = smsChannelA;
    }

    public void send() {
        this.smsChannel.send();
    }

    public SmsChannel getSmsChannel() {
        return smsChannel;
    }

    public void setSmsChannel(SmsChannel smsChannel) {
        this.smsChannel = smsChannel;
    }

    public SmsChannel getSmsChannelA() {
        return smsChannelA;
    }

    public SmsChannel getSmsChannelB() {
        return smsChannelB;
    }

    public SmsChannel getSmsChannelC() {
        return smsChannelC;
    }
}

public class Client {

    public static void main(String[] args) {
        SmsContext smsContext;
        for (int i = 0; i < 10; i++) {
            smsContext = new SmsContext();
            smsContext.send();
        }

    }
}
```

```
优点: 类分离，具体的类实现具体的功能
     具体的实现类具备先后顺序、即在编写代码的时候，就已经知道按照A->B-C
     的顺序进行发送
```



### 4: 与策略模式的区别:

状态模式需要在子类实现与context相关的一个状态行为,也就是说，状态模式的运转，是由自身的状态决定的,但是策略模式的任何一个策略是平等的，需要外部来进行选择其中的一个策略进行操作.

