# 1.5：在对象之间搬移特性

## 在对象之间搬移特性

#### 搬移函数(Move Method)

```
 实现高内聚,低耦合
```

#### 搬移字段(Move Field)

```
 当每某个字段被其他类更多的使用到的时候，需要搬移     
```

#### 提炼类(Extract Class)

```
 分解类所负的责任
```

#### 将类内联化(Inner Class)

```
 与Extract Class相反，如果一个类不再承担责任,就没有单独存在的理由      
```

#### 隐藏委托关系(Hide Delegate)

```
 在服务类上建立所需的函数
```

#### 移除中间人(Remove Middle Man)

```
  与Hide Delegate相反，关键看这东西是否可以开放
```

#### 加入外加函数(Introduce Foreign Method)

```
 原有的类，需要新增新的功能，但是又不能修改源码，使用新的类，提供新的外加函数
```

#### 引入本地扩展(Introduce Local Extension)

```
 建立一个新的类,使它包含这些额外参数                        
```