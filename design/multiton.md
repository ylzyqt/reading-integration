# 3.5：多例模式

多例模式倒不是无限制的new对象,通过初始化既定的对象，而实现重用的目的



### 1: 背景

```
连接池
```





### 2: 多例模式的做法

```java
public class ConnectionFactory {

    private String connectionName;

    public static List<ConnectionFactory> connectionFactoryList = new ArrayList<>();

    private static final Random random = new Random();

    static {
        connectionFactoryList.add(new ConnectionFactory("connection 1"));
        connectionFactoryList.add(new ConnectionFactory("connection 2"));
    }


    private static int randomIndex() {
        return random.nextInt(connectionFactoryList.size());
    }


    public static ConnectionFactory getInstance() {
        return connectionFactoryList.get(randomIndex());
    }


    private ConnectionFactory() {

    }

    public ConnectionFactory(String connectionName) {
        this.connectionName = connectionName;
    }

    public String getConnectionName() {
        return connectionName;
    }

    public void setConnectionName(String connectionName) {
        this.connectionName = connectionName;
    }
}
```

```
优点: 不会出现对象爆炸,可以实现对象重用
```

