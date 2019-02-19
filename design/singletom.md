# 3.4：单例模式

单例对象的类只能允许一个实例存在



### 1: 背景

```
代码中往往都有配置类，该配置类不希望有多个实例
```



### 2. 饿汉模式

饿了，不管三七二十一，先生成对象再说

```java
public class HungrySingleton {
    private static HungrySingleton hungrySingleton = new HungrySingleton();
    
    private HungrySingleton() {
    }
    public static HungrySingleton getInstance() {
        return hungrySingleton;
    }
}

```

```
优点: 特别建议，没有问题风险
```



### 3: 饱汉模式

```java
public class FullSingleton {
    private static FullSingleton fullSingleton;

    private FullSingleton() {

    }

    public synchronized FullSingleton getInstance() {
        if (fullSingleton == null) {
            fullSingleton = new FullSingleton();
        }
        return fullSingleton;
    }

    public FullSingleton getInstance_() {
        synchronized (FullSingleton.class) {
            if (fullSingleton == null) {
                fullSingleton = new FullSingleton();
            }
        }
        return fullSingleton;
    }

}
```

```
系统初始化的时候，不会生成FullSingleton对象，当使用到的时候，再创建这个对象.
注意，这种场景下有可能有并发问题，所以需要加锁
```

