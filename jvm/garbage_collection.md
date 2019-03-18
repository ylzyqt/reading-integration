# 4.6：垃圾回收机制

### 1: 概况

- [怎么判断对象已经变为垃圾?](garbage_judge.md)
  - 引用计数法
  - 可达性分析法
- [怎么回收?](garbage_collection_how.md)
  - 回收策略
    - 标记清除
    - 复制算法
    - 标记-整理算法
    - 分代收集算法
  - 垃圾回收器
    - serial
    - parnew
    - cms
    - G1
- [何时回收?](garbage_collection_when.md)