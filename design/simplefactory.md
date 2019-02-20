# 3.1：简单工厂

简单工厂模式就是一个工厂类根据传入的参量决定创建哪一种产品类的实例



### 1: 背景

```
出版社根据学校的需求印刷书籍
```





### 2: 简单工厂模式的做法

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

public class HistoryBook implements Book{
    @Override
    public void name() {
        System.out.println("历史书");
    }
}

public class MathBook implements Book{
    @Override
    public void name() {
        System.out.println("数学书");
    }
}

public class BookSimpleFactory {
    public Book create(String bookType) {
        if ("english".equalsIgnoreCase(bookType)) {
            return new EnglishBook();
        } else if ("math".equalsIgnoreCase(bookType)) {
            return new MathBook();
        } else if ("history".equalsIgnoreCase(bookType)) {
            return new HistoryBook();
        }
        return null;
    }
}

public class Client {
    public static void main(String[] args) {
        BookSimpleFactory factory = new BookSimpleFactory();
        factory.create("math").name();
        factory.create("history").name();
        factory.create("english").name();
    }
}

代码比较固定，根据传入的参数来决定生成什么对象
```

