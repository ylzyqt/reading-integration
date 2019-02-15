# 3.18：模版方法

定义一个操作中的算法的框架，而将一些步骤延迟到子类中



### 1: 背景

```
在基础项目中,任何数据库的操作,都是有基础的流程的，比如：
1: 写入/更新数据库
2: 记录日志
3: 刷新redis缓存
4: 发送kafka消息，以便关心数据变更的业务方及时获得到变更信息
```





### 2: 传统写法

```java
Class A 步骤:
1: 写入/更新数据库
2: 记录日志
3: 刷新redis缓存
4: 发送kafka消息，以便关心数据变更的业务方及时获得到变更信息

ClassB 步骤: 
1: 写入/更新数据库
2: 记录日志
3: 刷新redis缓存
4: 发送kafka消息，以便关心数据变更的业务方及时获得到变更信息
```

```
缺点: 逻辑关系是固定在，不管在何处，逻辑的先后，操作都是已经设置好的，变更的是里面具体的逻辑
```





### 3: 模版方法的写法

```java
/**
 * 模版方法,具体的流程逻辑已经在process中写好了
 */
public abstract class AbstractOperateTemplate {

    abstract boolean updateDataInDataBase();

    abstract void recordLog(boolean isUpdateSuccess);

    abstract boolean refreshRedis();

    abstract boolean sendNotifyMessageToKafka();


    public final void process() {
        boolean updateResultSuccess = updateDataInDataBase();
        recordLog(updateResultSuccess);
        if (updateResultSuccess) {
            refreshRedis();
            sendNotifyMessageToKafka();
        }
    }
}


/**
 * users表变更的具体实现
 */
public class UsersOperateTemplate extends AbstractOperateTemplate {
    @Override
    boolean updateDataInDataBase() {
        System.out.println("写入users信息到数据库");
        return true;
    }

    @Override
    void recordLog(boolean isUpdateSuccess) {
        System.out.println("写入users信息到数据库 结果:" + isUpdateSuccess);
    }

    @Override
    boolean refreshRedis() {
        System.out.println("刷新redis");
        return false;
    }

    @Override
    boolean sendNotifyMessageToKafka() {
        System.out.println("刷新kafka");
        return false;
    }
}


/**
 * 部门表变更的具体实现
 */
public class DepartmentsOperateTemplate extends AbstractOperateTemplate {
    @Override
    boolean updateDataInDataBase() {
        System.out.println("写入departments信息到数据库");
        return true;
    }

    @Override
    void recordLog(boolean isUpdateSuccess) {
        System.out.println("写入departments信息到数据库 结果:" + isUpdateSuccess);
    }

    @Override
    boolean refreshRedis() {
        System.out.println("刷新redis");
        return false;
    }

    @Override
    boolean sendNotifyMessageToKafka() {
        System.out.println("刷新kafka");
        return false;
    }
}



/**
 * 客户端调用方法
 */
public class Client {
    public static void main(String[] args) {
        AbstractOperateTemplate users = new UsersOperateTemplate();
        users.process();

        AbstractOperateTemplate departments = new DepartmentsOperateTemplate();
        departments.process();
    }
}


```

```
优点： 每个子类在写代码的时候,补充重复先后逻辑顺序,只需要填写自己代码中的逻辑即可。
```



