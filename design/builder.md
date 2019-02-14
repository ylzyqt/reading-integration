# 3.6：建造模式

将一个复杂的构建与其表示相分离，使得同样的构建过程可以创建不同的表示

比较适用的场景是,类的行为比较固定，类的属性不希望被重写



### 1: 背景

```
当A类调用B类的时候，如果A传递一个对象给B, 但是又不希望该对象的属性会发生变化,可以使用此模式解决
```



### 2: 传统做法

```java
public class OldBuilder implements Cloneable {
    private int id;
    private String name;
    private String address;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}
```

```
优点： 类比较简单,大部分的代码都可以由intellij idea自动生成,传递的时候，传递一个克隆对象过去，
无论对方怎么修改，不影响自身.
```



### 3. 建造模式做法

```java
public class NewBuilder {
    private int id;
    private String name;
    private String address;

    private NewBuilder(Builder builder) {
        this.id = builder.id;
        this.name = builder.name;
        this.address = builder.address;
    }

    public static Builder builder() {
        return new Builder();
    }

    static class Builder {
        private int id;
        private String name;
        private String address;

        public Builder id(int id) {
            this.id = id;
            return this;
        }

        public Builder name(String name) {
            this.name = name;
            return this;
        }

        public Builder address(String address) {
            this.address = address;
            return this;
        }

        public NewBuilder build() {
            return new NewBuilder(this);
        }
    }

    public int getId() {
        return id;
    }

    public String getName() {
        return name;
    }

    public String getAddress() {
        return address;
    }

    @Override
    public String toString() {
        return "NewBuilder{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", address='" + address + '\'' +
                '}';
    }
```

```
优点: 比较固定的类,建造之后,属性不可变 
缺点: 类比较复杂
```

