# 3.10：合成模式

合成模式把部分和整体的关系用树结构表示出来



### 1: 背景

```
超市购物，需要打印购物小票, 可见的组合如下
1: 顾客直接拿着 商品过来结账
2: 顾客购买的一部分商品，比如购买的吃食，已经在卖吃食的地方打包录入了系统了
```



### 2: 合成模式的做法

```java
/**
 * 接口类，封装具体的操作
 */
public interface Composite {

    void add(Composite composite);

    void remove(Composite composite);

    void printf();
}

/**
 * 商品类，顾客可以直接拿来结账的东西
 */
public class GoodsComposite implements Composite {

    private String name;
    private double price;


    public GoodsComposite(String name, double price) {
        this.name = name;
        this.price = price;
    }

    @Override
    public void add(Composite composite) {
        System.out.println("我是商品,不是购物车,不能新增商品");
    }

    @Override
    public void remove(Composite composite) {
        System.out.println("我是商品,不是购物车,不能移除商品");
    }

    @Override
    public void printf() {
        System.out.println("购买了:" + name + " 花费了:" + price);
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public double getPrice() {
        return price;
    }

    public void setPrice(double price) {
        this.price = price;
    }
}

/**
 * 购物车类，估计拿过来已经登记好在系统中的东西
 */
public class ShoppingCarComposite implements Composite {
    private List<Composite> composites = new ArrayList<>();


    @Override
    public void add(Composite composite) {
        composites.add(composite);
    }

    @Override
    public void remove(Composite composite) {
        composites.remove(composite);
    }

    @Override
    public void printf() {
        composites.forEach(composite -> composite.printf());
    }
}



/**
 * 客户端输出类
 */
public class Client {

    public static void main(String[] args) {
        GoodsComposite apple = new GoodsComposite("苹果", 10D);


        GoodsComposite bun = new GoodsComposite("包子", 5D);
        GoodsComposite dumplings = new GoodsComposite("饺子", 6D);
        GoodsComposite chaos = new GoodsComposite("混沌", 5D);

        ShoppingCarComposite eatComposite = new ShoppingCarComposite();
        eatComposite.add(bun);
        eatComposite.add(dumplings);
        eatComposite.add(chaos);

        ShoppingCarComposite all = new ShoppingCarComposite();
        all.add(apple);
        all.add(eatComposite);
        all.printf();

    }
}

输出结果:
购买了:苹果 花费了:10.0
购买了:包子 花费了:5.0
购买了:饺子 花费了:6.0
购买了:混沌 花费了:5.0

有点类似递归的实现，在ShoppingCarComposite中，封装的无论是ShoppingCarComposite
还是GoodsComposite,对系统来说，都是同样的调用,简化了系统行为
```

