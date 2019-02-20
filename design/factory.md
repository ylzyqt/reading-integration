# 3.2：工厂方法

通过定义工厂父类负责定义创建对象的公共接口，而子类则负责生成具体的对象

### 1: 背景

```
出版社根据学校的需求印刷书籍
```



### 2: 工厂方法的实现

实现的时候,不再是一个工厂类占据最主要的位置了,而是通过工厂实现类来创建对象

```java
public interface Book {
    void name();
}

public class EnglishBook implements Book {
    @Override
    public void name() {
        System.out.println("英语书");
    }
}

public class HistoryBook implements Book {
    @Override
    public void name() {
        System.out.println("历史书");
    }
}

public class MathBook implements Book {
    @Override
    public void name() {
        System.out.println("数学书");
    }
}


public interface Factory {
    Book createBook();
}

public class EnglishBookFactory implements Factory{
    @Override
    public Book createBook() {
        return new EnglishBook();
    }
}

public class HistoryBookFactory implements Factory{
    @Override
    public Book createBook() {
        return new HistoryBook();
    }
}

public class MathBookFactory implements Factory {
    @Override
    public Book createBook() {
        return new MathBook();
    }
}

public class Client {

    public static void main(String[] args) {

        Factory englishBookFactory = new EnglishBookFactory();
        englishBookFactory.createBook().name();

        Factory historyBookFactory = new HistoryBookFactory();
        historyBookFactory.createBook().name();

        Factory mathBookFactory = new MathBookFactory();
        mathBookFactory.createBook().name();
    }
}
```

