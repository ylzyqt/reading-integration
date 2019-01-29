# 1.8: 简化函数调用

#### 1 .函数改名

```
将复杂的处理过程分成诸多小函数,但是这些小函数需要有比较好的命名,否则会出现混乱

getNumber -> getOfficeTelephoneNumber
```

#### 2. 以明确函数取代参数

```
用明确的函数取代参数的方式,可以使代码的可读性更高

getSomethingByType(int status);
-->
getSomethingByDone();
getSomethingByReady();
```



#### 3. 保持对象的完整

```
传递调用的时候，用对象替换具体的参数，可以应对后面的一些调整
Room room = new Room();
room.setId(1);
room.setRoomNum(168);
room.setRoomType("大床房");

public double calculateRoomPrice(room.getId,room.getRoomNum);
-->
public double calculateRoomPrice(room);
```



#### 4 . 过长的参数列表

````
传递参数的时候,如果参数太长，那么需要调整
getRoom(String roomName,String roomType,String roomAddress....);

->
getRoom(Room room);
````



#### 5. 参数不可变

```
对象中,一旦某个值设置之后就不可以变更,那么需要标明为final
并且，取消其set函数
```



#### 6.隐藏函数

````
当某个类,提供了函数供外部调用的时候，特别要注意,部分不需要透露的函数，一定要注意其可见性
````



#### 7. 以工厂函数替换构造函数

```
创建类的时候,一般使用构造函数的方式如下:
Factory fac = new Factory(1);
->
Factory fac = Factory.create(1);
```



#### 8.以异常取代错误码

```
程序内部调用的时候，直接可以以异常来替换错误码
但是对于api层面,需要对此进行封装，以免暴露过多的内部信息
```

