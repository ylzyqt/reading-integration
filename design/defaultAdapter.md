# 3.9：缺省适配模式

缺省适配器模式是由适配器模式简化而来，省略了适配器模式中目标接口，也就是源接口和目标接口相同，源接口为接口，目标接口为类

###  1: 背景

 ```
在接受表内容变更消息的时候,有insert,update,delete事件,不同的表的表更，需要对应的处理策略
user表，只需要处理新增和变更
department表,需要处理新增删除
 ```



### 2: 传统做法

```java
public interface TableChangeEventService {

    void insert(String tableName);

    void update(String tableName);

    void delete(String tableName);
}


/**
 * user表变更,里面只处理新增，修改的地方，删除的地方，代码则不处理
 */
public class UserTableChangeEventServiceImpl implements TableChangeEventService {
    @Override
    public void insert(String tableName) {
        System.out.println(" user 新增");
    }

    @Override
    public void update(String tableName) {
        System.out.println(" user 修改");
    }

    @Override
    public void delete(String tableName) {
        // do nothing
    }
}

/**
 * departments表变更,里面只处理删除的地方，其他的地方，代码则不处理
 */
public class DepartmentTableChangeEventServiceImpl implements TableChangeEventService {
    @Override
    public void insert(String tableName) {
        // do nothing
    }

    @Override
    public void update(String tableName) {
        // do nothing
    }

    @Override
    public void delete(String tableName) {
        System.out.println("departments 删除");
    }
}

```

```
缺点: 可以看到,尽管insert ,update ,delete 都是表内容的变更行为,但是很明显,
实现类并不关心所有的事件行为,导致实现类里面必须实现一部分代码，然后这部分代码又
没有什么意义
```



### 3:  缺省适配器的做法

```java
/**
 * 新增适配器类,该适配器类给了一个默认的空的实现
 */
public class TableChangeEventServiceAdpater implements TableChangeEventService {

    @Override
    public void insert(String tableName) {
        // do nothing
    }

    @Override
    public void update(String tableName) {
        // do nothing
    }

    @Override
    public void delete(String tableName) {
        // do nothing
    }
}

/**
 * 用户表的变更,只需要关注insert 和update即可
 */
public class UserTableChangeEventServiceImpl extends TableChangeEventServiceAdpater {
    @Override
    public void insert(String tableName) {
        System.out.println(" user 新增");
    }

    @Override
    public void update(String tableName) {
        System.out.println(" user 修改");
    }
}

/**
 * 部门表的变更,只需要关注删除即可
 */
public class DepartmentTableChangeEventServiceImpl extends TableChangeEventServiceAdpater {

    @Override
    public void delete(String tableName) {
        System.out.println("departments 删除");
    }
}

```

