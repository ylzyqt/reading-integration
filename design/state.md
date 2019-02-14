# 3.24：状态模式

允许一个对象在其内部状态改变时改变它的行为。对象看起来似乎修改了它的类

特点: 有多个行为,根据状态来进行判断

### 1: 业务背景

```
当业务字段的变更,需要通知到各个业务的时候,需要发送消息队列,发送的时候,需要可能有多个消息队列
- kafka
- rabbitmq
- activemq
```



### 2: 传统做法

```java
public class OldSender {
    public static final int KAFKA = 0;
    public static final int RABBITMQ = 1;
    public static final int ACTIVEMQ = 2;

    public static void main(String[] args) {
        OldSender oldSender = new OldSender();
        oldSender.send(KAFKA);
    }
    
    public void send(int sendType) {
        switch (sendType) {
            case KAFKA:
                System.out.println("发送Kafka消息");
                break;
            case RABBITMQ:
                System.out.println("发送RabbitMq消息");
                break;
            case ACTIVEMQ:
                System.out.println("发送activeMq消息");
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
/***
 * 抽象父类,用以表面发送的行为
 **/
public abstract class State {
    StateContext stateContext;
    public State(StateContext stateContext) {
        this.stateContext = stateContext;
    }
    abstract void send();
}

/***
 * 发送kafka 的具体实践类
 **/
public class StateOfKafka extends State {
    public StateOfKafka(StateContext stateContext) {
        super(stateContext);
    }
    @Override
    public void send() {
        if (ConfigUtils.isKafakEnable()) {
            System.out.println("发送Kafka消息");
        } else {
            stateContext.setState(stateContext.stateOfRabbitMq);
            stateContext.send();
        }

    }
}

/***
 * 发送rabbitmq的具体实现类
 **/
public class StateOfRabbitMq extends State {
    public StateOfRabbitMq(StateContext stateContext) {
        super(stateContext);
    }
    @Override
    public void send() {
        if(ConfigUtils.isRabbitMqEnable()){
            System.out.println("发送RabbitMq消息");
        }else{
            stateContext.setState(stateContext.stateOfActiveMq);
            stateContext.send();

        }

    }
}

/***
 * 发送activemq的具体实现类
 **/
public class StateOfActiveMq extends State {
    public StateOfActiveMq(StateContext stateContext) {
        super(stateContext);
    }
    @Override
    public void send() {
        if (ConfigUtils.isActiveMqEnable()) {
            System.out.println("发送activeMq消息");
        } else {
            stateContext.setState(stateContext.stateOfNoNe);
            stateContext.send();
        }

    }
}

/***
 * 未知发送对象类
 **/
public class StateOfNone extends State {
    public StateOfNone(StateContext stateContext) {
        super(stateContext);
    }

    @Override
    public void send() {
        System.out.println("未知发送渠道");
    }
}

/***
 * 控制类
 **/
public class StateContext {
    private State state;
    public State stateOfKafka = new StateOfKafka(this);
    public State stateOfActiveMq = new StateOfActiveMq(this);
    public State stateOfRabbitMq = new StateOfRabbitMq(this);
    public State stateOfNoNe = new StateOfNone(this);

    public void send() {
        state.send();
    }

    public void setState(State state) {
        this.state = state;
    }

    public State getStateOfKafka() {
        return stateOfKafka;
    }

    public State getStateOfActiveMq() {
        return stateOfActiveMq;
    }

    public State getStateOfRabbitMq() {
        return stateOfRabbitMq;
    }

    public State getStateOfNoNe() {
        return stateOfNoNe;
    }
}

/***
 * 调用类
 **/
public class Client {
    public static void main(String[] args) {
        StateContext stateContext = new StateContext();
        stateContext.setState(stateContext.getStateOfKafka());
        stateContext.send();
    }
}

/***
 * 配置类,该类可以从配置中心、从外部配置中获取
 **/
public class ConfigUtils {

    public static boolean isKafakEnable() { return false; }

    public static boolean isRabbitMqEnable() { return false;}

    public static boolean isActiveMqEnable() { return false;}
}

```

```
优点: 类分离，具体的类实现具体的功能
     具体的实现类具备先后顺序、即在编写代码的时候，就已经知道按照kafka-rabbitmq-activemq
     的顺序进行发送
```



### 4: 与策略模式的区别:

状态模式需要在子类实现与context相关的一个状态行为,也就是说，状态模式的运转，是由自身的状态决定的,但是策略模式的任何一个策略是平等的，需要外部来进行选择其中的一个策略进行操作.

