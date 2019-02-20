# 3.23：备忘录模式

在不破坏封装的前提下,捕获一个对象的内部状态,并在该对象之外保存这个状态,
这样以后就可以将该对象恢复至原先保存的状态

### 1: 背景

```
有些比较重要的系统参数,为了防止错误的修改，造成错误影响，都需要回滚功能，比如在k8s中，
有资源限制的参数,但是，资源限制一旦修改除了问题，需要快速回滚到上个版本
```





### 2: 备忘录的实现

```java
/**
 * 配置类
 */
public class Config implements Cloneable {

    private String deploymentName;

    private String nodeSelector;

    private int resourceMinMemory;

    private int resourceMaxMemory;

    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }

    public String getDeploymentName() {
        return deploymentName;
    }

    public void setDeploymentName(String deploymentName) {
        this.deploymentName = deploymentName;
    }

    public String getNodeSelector() {
        return nodeSelector;
    }

    public void setNodeSelector(String nodeSelector) {
        this.nodeSelector = nodeSelector;
    }

    public int getResourceMinMemory() {
        return resourceMinMemory;
    }

    public void setResourceMinMemory(int resourceMinMemory) {
        this.resourceMinMemory = resourceMinMemory;
    }

    public int getResourceMaxMemory() {
        return resourceMaxMemory;
    }

    public void setResourceMaxMemory(int resourceMaxMemory) {
        this.resourceMaxMemory = resourceMaxMemory;
    }

    @Override
    public String toString() {
        return "Config{" +
                "deploymentName='" + deploymentName + '\'' +
                ", nodeSelector='" + nodeSelector + '\'' +
                ", resourceMinMemory='" + resourceMinMemory + '\'' +
                ", resourceMaxMemory='" + resourceMaxMemory + '\'' +
                '}';
    }
}

/**
 * 备忘录类，在这个类里面,进行备份工作
 */
public class ConfigMemento {
    private static int currentIndex = 0;
    private List<Config> configs = new ArrayList<>();


    public void backUp(Config config) throws CloneNotSupportedException {
        configs.add((Config) config.clone());
        currentIndex = configs.size() - 1;
    }

    public Config restore() {
        if (configs.size() < currentIndex) {
            System.out.println("未备份,无法恢复");
            return null;
        }
        Config con = configs.get(currentIndex);
        currentIndex--;
        return con;
    }
}


/**
 * 客户端类,在此类中验证备份、回滚的操作
 */
public class Client {

    public static void main(String[] args) throws Exception {
        Config config = new Config();
        ConfigMemento memento = new ConfigMemento();
        config.setDeploymentName("memento_test");
        config.setNodeSelector("memento");
        config.setResourceMinMemory(128);
        config.setResourceMaxMemory(1024);

        System.out.println("最原始的样子: " + config);
        memento.backUp(config);
        config.setResourceMaxMemory(2048);
        System.out.println("第一次修改后: " + config);
        memento.backUp(config);

        config.setResourceMaxMemory(3000);
        System.out.println("第二次修改后: " + config);


        config = memento.restore();

        System.out.println("恢复到第一次: " + config);

        config = memento.restore();

        System.out.println("恢复到最原始: " + config);

    }
}


最原始的样子: Config{deploymentName='memento_test', nodeSelector='memento', 
               resourceMinMemory='128', resourceMaxMemory='1024'}
第一次修改后: Config{deploymentName='memento_test', nodeSelector='memento',
               resourceMinMemory='128', resourceMaxMemory='2048'}
第二次修改后: Config{deploymentName='memento_test', nodeSelector='memento', 
               resourceMinMemory='128', resourceMaxMemory='3000'}
恢复到第一次: Config{deploymentName='memento_test', nodeSelector='memento', 
               .resourceMinMemory='128', resourceMaxMemory='2048'}
恢复到最原始: Config{deploymentName='memento_test', nodeSelector='memento', 
               resourceMinMemory='128', resourceMaxMemory='1024'}

从输出结果看,可以快速回滚到上一个版本,在业务中使用的时候，可以罗列每个版本具体的信息,
便于选择，然后快速回滚到指定的版本
```

