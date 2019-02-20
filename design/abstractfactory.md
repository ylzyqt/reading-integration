# 3.3：抽象工厂

抽象工厂模式提供一个接口，用于创建相关或依赖对象的家族，而不需要明确指定具体类。可以理解成是多个工厂方法的组合

### 1: 背景

```
在工厂方法中,已经使用了工厂方法的模式进行书籍的生产,现在情况更升一级，现在需要印刷不同年级的书籍
```



### 2: 抽象工厂

```java
public interface Grade {
    void createGradeBook(int grade);
}


public class GradeFactory implements Grade{
    @Override
    public void createGradeBook(int grade) {
        System.out.println("开始印刷: " + grade+" 年级的书籍");

        new EnglishBookFactory().createBook().name();
        new HistoryBookFactory().createBook().name();
        new MathBookFactory().createBook().name();

        System.out.println("结束印刷: " + grade+" 年级的书籍");
    }
}

public class Client {
    public static void main(String[] args){
        Grade grade = new GradeFactory();
        grade.createGradeBook(1);
    }
}
```

