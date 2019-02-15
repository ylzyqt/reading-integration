# 3.7：原始模式

通过一个原型对象来指明要创建对象的类型，然后用复制原型对象的方法来创建出更多同类型的对象



### 1 : 背景

```
在某个APP的九宫格展示的过程中,九宫格里面的数据来自与数据库,为了提高接口的速度，
希望将数据库中的内容都缓存到内存中,然后接口请求的过程中，都从内存中复制一份，根据请求参数
过滤一下，将过滤后的九宫格返回给前端， 注意: 存储在内存中的九宫格和 代码中返回的不是一份
```





### 2: 错误的做法

```java
public class Module {
    private int redDotNumber = 0;
    private List<String> modules = new ArrayList<String>() {{
        add("用户管理");
        add("系统设置");
        add("提醒设置");
    }};
    public int getRedDotNumber() {
        return redDotNumber;
    }
    public void setRedDotNumber(int redDotNumber) {
        this.redDotNumber = redDotNumber;
    }
    public List<String> getModules() {
        return modules;
    }
    public void setModules(List<String> modules) {
        this.modules = modules;
    }
    @Override
    public String toString() {
        return "Module{" +
                "redDotNumber=" + redDotNumber +
                ", modules=" + modules +
                '}';
    }
}


public class Client {

    static Module module = new Module();

    public static void main(String[] args) {

        System.out.println("客户端1 接口开始请求模块数据");

        Module currentModule = module;
        System.out.println(currentModule == module);
        currentModule.setRedDotNumber(10);

        System.out.println("客户端1 请求结果:" + currentModule);

        System.out.println("******************** ");

        System.out.println("客户端2 接口开始请求模块数据");

        currentModule = module;

        System.out.println("客户端2 请求结果:" + currentModule);

    }
}

输出结果为:

客户端1 接口开始请求模块数据
true
客户端1 请求结果:Module{redDotNumber=10, modules=[用户管理, 系统设置, 提醒设置]}
******************** 
客户端2 接口开始请求模块数据
客户端2 请求结果:Module{redDotNumber=10, modules=[用户管理, 系统设置, 提醒设置]}

但是结果跟预想的不一样:
对于客户端1 ,从Module拿值，设置了红点需要展示10
对于客户端2 ,因为没有红点,并没有设置红点，但是从输出的内容来看，其红点值也变成了10

原因在于客户端1在访问的时候，采用的方式是:
Module currentModule = module;
对currentModule的修改,即是对module的修改
```





### 3: 原始模式的做法

```java
public class Module implements Cloneable {

    private int redDotNumber = 0;

    private List<String> modules = new ArrayList<String>() {{
        add("用户管理");
        add("系统设置");
        add("提醒设置");
    }};


    public int getRedDotNumber() {
        return redDotNumber;
    }

    public void setRedDotNumber(int redDotNumber) {
        this.redDotNumber = redDotNumber;
    }

    public List<String> getModules() {
        return modules;
    }

    public void setModules(List<String> modules) {
        this.modules = modules;
    }

    @Override
    protected Module clone() throws CloneNotSupportedException {
        return (Module) super.clone();
    }

    @Override
    public String toString() {
        return "Module{" +
                "redDotNumber=" + redDotNumber +
                ", modules=" + modules +
                '}';
    }
}


public class Client {

    static Module module = new Module();

    public static void main(String[] args) throws CloneNotSupportedException {

        System.out.println("客户端1 接口开始请求模块数据");

        Module currentModule = module.clone();
        System.out.println(currentModule == module);

        currentModule.setRedDotNumber(10);

        System.out.println("客户端1 请求结果:" + currentModule);

        System.out.println("******************** ");

        System.out.println("客户端2 接口开始请求模块数据");

        currentModule = module.clone();

        System.out.println("客户端2 请求结果:" + currentModule);

    }
}

客户端1 接口开始请求模块数据
false
客户端1 请求结果:Module{redDotNumber=10, modules=[用户管理, 系统设置, 提醒设置]}
******************** 
客户端2 接口开始请求模块数据
客户端2 请求结果:Module{redDotNumber=0, modules=[用户管理, 系统设置, 提醒设置]}

从输出的结果看，每个客户端请求过来的时候,拿到的都是Module的副本，对副本的改动不影响原先的对象.

```





### 4: 浅拷贝的误区

```java
修改一下Client代码:
public class Client {

    static Module module = new Module();

    public static void main(String[] args) throws CloneNotSupportedException {

        System.out.println("客户端1 接口开始请求模块数据");

        Module currentModule = module.clone();
        System.out.println(currentModule == module);

        currentModule.setRedDotNumber(10);
        currentModule.getModules().remove("用户管理"); //新增了这句话

        System.out.println("客户端1 请求结果:" + currentModule);

        System.out.println("******************** ");

        System.out.println("客户端2 接口开始请求模块数据");

        currentModule = module.clone();

        System.out.println("客户端2 请求结果:" + currentModule);

    }
}
客户端1 接口开始请求模块数据
false
客户端1 请求结果:Module{redDotNumber=10, modules=[系统设置, 提醒设置]}
******************** 
客户端2 接口开始请求模块数据
客户端2 请求结果:Module{redDotNumber=0, modules=[系统设置, 提醒设置]}

靠,Module里面的list内容被改变了，虽然redDotNumber是正确的,这种情况，就需要考虑深拷贝


```



### 5.深拷贝

```java
public class Module implements Cloneable {

    private int redDotNumber = 0;

    private List<String> modules = new ArrayList<String>() {{
        add("用户管理");
        add("系统设置");
        add("提醒设置");
    }};

    private List<String> copyModules;

    public int getRedDotNumber() {
        return redDotNumber;
    }

    public void setRedDotNumber(int redDotNumber) {
        this.redDotNumber = redDotNumber;
    }

    public List<String> getModules() {
        return modules;
    }

    public void setModules(List<String> modules) {
        this.modules = modules;
    }

    @Override
    protected Module clone() throws CloneNotSupportedException {
        return (Module) super.clone();
    }

    protected Module cloneDeep() throws CloneNotSupportedException {
        Module module = (Module) super.clone();
        module.setModules(copyOfModules());
        return module;
    }

    private List<String> copyOfModules() {
        copyModules = new ArrayList<>(modules.size());
        copyModules.addAll(modules);
        return copyModules;
    }


    @Override
    public String toString() {
        return "Module{" +
                "redDotNumber=" + redDotNumber +
                ", modules=" + modules +
                '}';
    }
}


public class Client {

    static Module module = new Module();

    public static void main(String[] args) throws CloneNotSupportedException {

        System.out.println("客户端1 接口开始请求模块数据");

        Module currentModule = module.cloneDeep();
        System.out.println(currentModule == module);

        currentModule.setRedDotNumber(10);
        currentModule.getModules().remove("用户管理");

        System.out.println("客户端1 请求结果:" + currentModule);

        System.out.println("******************** ");

        System.out.println("客户端2 接口开始请求模块数据");

        currentModule = module.cloneDeep();

        System.out.println("客户端2 请求结果:" + currentModule);

    }
}

客户端1 接口开始请求模块数据
false
客户端1 请求结果:Module{redDotNumber=10, modules=[系统设置, 提醒设置]}
******************** 
客户端2 接口开始请求模块数据
客户端2 请求结果:Module{redDotNumber=0, modules=[用户管理, 系统设置, 提醒设置]}

此时，输出的结果看，已经达到了目标，每个客户端发起的请求，拿到的都是Module的一个副本，互相之间是不影响
```

