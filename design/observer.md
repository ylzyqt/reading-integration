# 3.19：观察者模式

一个目标物件管理所有相依于它的观察者物件，并且在它本身的状态改变时主动发出通知



### 1: 背景

```
在一个业务系统中，系统A负责一个基础数据库的维护,里面有数据表
users,departments,compays表，这些表的内容一旦发生变更，需要通知到关系的其他系统,
现有的模式下，就是发送变更消息到kafka,其他业务来监听kafka消息,简单的可以用如下的
观察者模式来表示
```



### 2: 观察者模式的实现方式

```java
/**
 * 表结构变更的父类
 */
public abstract class TableEvent {
    Map<String, List<TableObserver>> map = new HashMap<>();

    abstract String tableName();

    void addObserver(TableObserver tableObserver) {
        List<TableObserver> tableObservers = getObservers();
        tableObservers.add(tableObserver);
        map.put(tableName(), tableObservers);
    }

    void removeObserver(TableObserver tableObserver) {
        List<TableObserver> tableObservers = getObservers();
        tableObservers.remove(tableObserver);
        map.put(tableName(), tableObservers);

    }

    void notifyTableEvent(String event) {
        List<TableObserver> tableObservers = getObservers();
        tableObservers.forEach(x -> x.revive(event));
    }

    List<TableObserver> getObservers() {
        List<TableObserver> tableObservers = map.get(tableName());
        if (tableObservers == null) {
            tableObservers = new ArrayList<>();
        }
        return tableObservers;
    }
}

/**
 * users表表更
 */
public class UsersTableEvent extends TableEvent {
    @Override
    public String tableName() {
        return "users";
    }
}

/**
 * Companys表表更
 */
public class CompanysTableEvent extends TableEvent {
    @Override
    public String tableName() {
        return "companys";
    }
}

/**
 * Departments表表更
 */
public class DepartmentsTableEvent extends TableEvent {
    @Override
    public String tableName() {
        return "departments";
    }
}

/**
 * 观察者类
 */
public interface TableObserver {

    void register(TableEvent tableEvent);

    void remove(TableEvent tableEvent);

    void revive(String event);
}

/**
 * 其中的一个系统监听者
 */
public class SystemBObserver implements TableObserver {

    @Override
    public void register(TableEvent tableEvent) {
        tableEvent.addObserver(this);
        System.out.println("SystemB 开始监听表:" + tableEvent.tableName());
    }

    @Override
    public void remove(TableEvent tableEvent) {
        tableEvent.removeObserver(this);
        System.out.println("SystemB 取消监听表:" + tableEvent.tableName());
    }

    @Override
    public void revive(String event) {
        System.out.println("SystemB 监听到了:" + event);
    }
}

/**
 * 客户端使用的情况
 */
public class Client {

    public static void main(String[] args) {

        TableEvent usersTableEvent = new UsersTableEvent();
        TableEvent departmentsEvent = new DepartmentsTableEvent();
        TableEvent companyEvent = new CompanysTableEvent();


        SystemBObserver observer = new SystemBObserver();

        observer.register(usersTableEvent);
        observer.register(departmentsEvent);
        observer.register(companyEvent);

        companyEvent.notifyTableEvent("compay发送了表数据删除1");

        observer.remove(companyEvent);

        companyEvent.notifyTableEvent("compay发送了表数据删除2");

    }
}

输出结果:
SystemB 开始监听表:users
SystemB 开始监听表:departments
SystemB 开始监听表:companys
SystemB 监听到了:compay发送了表数据删除1
SystemB 取消监听表:companys
```

```
优点: 代码结构非常清晰，具体的操作部分，只负责自己的操作，没有太多的关联操作代码
```

