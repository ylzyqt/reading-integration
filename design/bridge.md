# 3.15：桥梁模式

**将抽象和实现解耦，使得两者可以独立的变化**

### 1: 背景

```
有3个画笔(大,中，小), 5个颜色(红黄绿蓝橙), 构造出 画出不同粗细、不同颜色的画 
```





### 2: 桥梁模式的实现

```java
public interface Color {
    String color();
}

public class RedColor implements Color {
    @Override
    public String color() {
        return " 蘸了红色墨水 ";
    }
}

public class YellowColor implements Color {
    @Override
    public String color() {
        return " 蘸了黄色墨水 ";
    }
}

public class BlueColor implements Color {
    @Override
    public String color() {
        return " 蘸了蓝色墨水 ";
    }
}

public class GreenColor implements Color {
    @Override
    public String color() {
        return " 蘸了绿色墨水 ";
    }
}

public class OrangeColor implements Color {
    @Override
    public String color() {
        return " 蘸了橙色墨水 ";
    }
}

/**
 * 画笔,用来作画
 */
public abstract class Pen {

    private Color color;

    public Pen(Color color) {
        this.color = color;
    }

    abstract void draw();

    public Color getColor() {
        return color;
    }

    public void setColor(Color color) {
        this.color = color;
    }
}

public class BigPen extends Pen {

    public BigPen(Color color) {
        super(color);
    }

    @Override
    public void draw() {
        System.out.println(" 用了大号笔," + getColor().color());
    }
}

public class BigPen extends Pen {

    public BigPen(Color color) {
        super(color);
    }

    @Override
    public void draw() {
        System.out.println(" 用了大号笔," + getColor().color());
    }
}

public class SmallPen extends Pen {
    public SmallPen(Color color) {
        super(color);
    }

    @Override
    public void draw() {
        System.out.println(" 用了小号笔 " + getColor().color());
    }
}


public class Client {
    private static Color red = new RedColor();
    private static Color yellow = new YellowColor();
    private static Color orange = new OrangeColor();
    private static Color green = new GreenColor();
    private static Color blue = new BlueColor();


    public static void main(String[] args) {

        BigPen bigPen = new BigPen(red);
        bigPen.draw();

        BigPen bigPenYellow = new BigPen(yellow);
        bigPenYellow.draw();

    }
}
```

```
如上面所示，原本需要15个实现类，才能够完成的工作，用了8个实现类就可以了,而且随着颜色的增加,
并不会出现类的爆炸性扩展
```

