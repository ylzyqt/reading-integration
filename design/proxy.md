# 3.12：代理模式

就是对已经存在的某些类进行代理，替代其做一些功能

### 1 : 背景

```
嗯.... 
想买学区房,给娃上学, 自己买？ 中介买？
```



```java
/**
 *买学区房接口
 **/
public interface IBuySchoolDistrictRoomService {
    void buySchoolDistrictRoom();
}

/**
 * 买学区房的具体步骤
 */
public class BuySchoolDistrictRoomServiceImpl implements IBuySchoolDistrictRoomService {
    @Override
    public void buySchoolDistrictRoom() {
        System.out.println("产权调查....");
        System.out.println("水、电、煤 检查....");
        System.out.println("学区房是否已用检查....");
        System.out.println("办理过户...");
        System.out.println("办理户口...");
    }
}
```



### 2: 自己买房

```java
public class Client {

    public static void main(String[] args) {
        IBuySchoolDistrictRoomService myself = new BuySchoolDistrictRoomServiceImpl();
        //自己去买
        System.out.println("买房前准备......");
        myself.buySchoolDistrictRoom();
        System.out.println("买房后装修......");

    }
}

可以看到,无论是 买房的任何一个步骤，都需要myself自己来做
```



### 3: 找中介买房 静态代理的实现

```java
/**
 * 找个中介realEstateAgents
 * 来帮我做具体买房的事情,但是
 * 买房前准备: 比如筹钱、选择学区
 * 买房后装修: 装修是自己的事情，房产中介不做
 **/
public class BuySchoolDistrictRoomProxy implements IBuySchoolDistrictRoomService {

    private IBuySchoolDistrictRoomService realEstateAgents;

    public BuySchoolDistrictRoomProxy(IBuySchoolDistrictRoomService realEstateAgents) {
        this.realEstateAgents = realEstateAgents;
    }

    @Override
    public void buySchoolDistrictRoom() {
        System.out.println("买房前准备.....");
        realEstateAgents.buySchoolDistrictRoom();
        System.out.println("买房后装修......");
    }
}

/**
 * 中介帮我买房子
 */
public class Client1 {

    public static void main(String[] args) {
        IBuySchoolDistrictRoomService myself = new BuySchoolDistrictRoomServiceImpl();

        IBuySchoolDistrictRoomService proxy = new BuySchoolDistrictRoomProxy(myself);
        //找中介去买
        proxy.buySchoolDistrictRoom();
    }
}
```





### 4: 找哪个中介买房 动态代理的实现

```java
/**
 * 动态代理类
 */
public class Proxy implements InvocationHandler {

    private Object object;

    public Proxy(Object object) {
        this.object = object;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println("买房前准备......");

        Object result = method.invoke(object, args);

        System.out.println("买房后装修......");

        return result;
    }
}


/**
 * lianjia 中介
 */
public class LianJiaBuySchoolDistrictRoomServiceImpl implements  
    IBuySchoolDistrictRoomService {
    @Override
    public void buySchoolDistrictRoom() {
        System.out.println("产权调查....");
        System.out.println("水、电、煤 检查....");
        System.out.println("学区房是否已用检查....");
        System.out.println("办理过户...");
        System.out.println("办理户口...");
        System.out.println("全程提供咖啡");
    }
}


/**
 * woaiwojia 中介
 */
public class WoAiWoJiaBuySchoolDistrictRoomServiceImpl implements 
    IBuySchoolDistrictRoomService {
    @Override
    public void buySchoolDistrictRoom() {
        System.out.println("产权调查....");
        System.out.println("水、电、煤 检查....");
        System.out.println("学区房是否已用检查....");
        System.out.println("办理过户...");
        System.out.println("办理户口...");
        System.out.println("中介费便宜1万");
    }
}



/**
 * 测试类,分别创建了2个proxy
 */
public class DynamicTest {

    public static void main(String[] args) {
        IBuySchoolDistrictRoomService lianjia = 
                new LianJiaBuySchoolDistrictRoomServiceImpl();
        IBuySchoolDistrictRoomService woaiwojia = 
                new WoAiWoJiaBuySchoolDistrictRoomServiceImpl();

        IBuySchoolDistrictRoomService lianjiaProxy = (IBuySchoolDistrictRoomService)
                java.lang.reflect.Proxy.newProxyInstance(
                        IBuySchoolDistrictRoomService.class.getClassLoader(),
                        new Class[]{IBuySchoolDistrictRoomService.class},
                        new Proxy(lianjia)
                );

        IBuySchoolDistrictRoomService woaiwojiaProxy = (IBuySchoolDistrictRoomService)
                java.lang.reflect.Proxy.newProxyInstance(
                        IBuySchoolDistrictRoomService.class.getClassLoader(),
                        new Class[]{IBuySchoolDistrictRoomService.class},
                        new Proxy(woaiwojia)
                );

        lianjiaProxy.buySchoolDistrictRoom();

        System.out.println("-------------------------------");

        woaiwojiaProxy.buySchoolDistrictRoom();
    }
}

嗯,可以看到具体的中介能干什么了,嗯想找个便宜的....

    
    
/**
 * 有了2个中介，一个是lianjia,一个是woaiwojia
 * 刚开始其实不知道该找谁，不过有个要求，谁便宜就是谁
 * getCheaper方法帮我判断出了woaiwojia更便宜,但是这一些，
 * 都是动态的,当某天lianjia便宜1.5万的时候,可能就不一样了 
 */    
public class DynamicTest {

    public static void main(String[] args) {
        IBuySchoolDistrictRoomService lianjia =
                new LianJiaBuySchoolDistrictRoomServiceImpl();
        IBuySchoolDistrictRoomService woaiwojia =
                new WoAiWoJiaBuySchoolDistrictRoomServiceImpl();

        IBuySchoolDistrictRoomService cheaper = getCheaper(lianjia, woaiwojia);
        cheaper.buySchoolDistrictRoom();
    }

    private static IBuySchoolDistrictRoomService getCheaper(
            IBuySchoolDistrictRoomService... args) {
        IBuySchoolDistrictRoomService service = null;

        for (IBuySchoolDistrictRoomService proxy : args) {
            if (proxy instanceof WoAiWoJiaBuySchoolDistrictRoomServiceImpl) {
                System.out.println("woaiwojia好像便宜些,就选他......");
                service = proxy;
            }
        }
        return buildProxy(service);

    }


    private static IBuySchoolDistrictRoomService buildProxy(
            IBuySchoolDistrictRoomService service) {
        return (IBuySchoolDistrictRoomService)
                java.lang.reflect.Proxy.newProxyInstance(
                        IBuySchoolDistrictRoomService.class.getClassLoader(),
                        new Class[]{IBuySchoolDistrictRoomService.class},
                        new Proxy(service)
                );
    }

}
```



