# 4.5.2：对象优先在Eden分配

### 1: 代码示例

```java
public class MemoryEden {
    public static void main(String[] args) {
        byte[] bytes1 = new byte[10 * 1024 * 1024];
        byte[] bytes2 = new byte[10 * 1024 * 1024];
        byte[] bytes3 = new byte[10 * 1024 * 1024];
        byte[] bytes4 = new byte[10 * 1024 * 1024];
    }
}
```



### 2: jvm 运行参数

```
-verbose:gc -XX:+PrintGCDetails -Xms50M -Xmx50M 
```



### 3: 运行结果

```
Heap
 PSYoungGen      total 14848K, used 12598K [0x00000007bef80000, 0x00000007c0000000, 0x00000007c0000000)
  eden space 12800K, 98% used [0x00000007bef80000,0x00000007bfbcdbd0,0x00000007bfc00000)
  from space 2048K, 0% used [0x00000007bfe00000,0x00000007bfe00000,0x00000007c0000000)
  to   space 2048K, 0% used [0x00000007bfc00000,0x00000007bfc00000,0x00000007bfe00000)
 ParOldGen       total 34304K, used 30720K [0x00000007bce00000, 0x00000007bef80000, 0x00000007bef80000)
  object space 34304K, 89% used [0x00000007bce00000,0x00000007bec00030,0x00000007bef80000)
 Metaspace       used 3307K, capacity 4496K, committed 4864K, reserved 1056768K
  class space    used 365K, capacity 388K, committed 512K, reserved 1048576K
```



### 4: 分析

```
1: 对象优先在Eden分配
2: 当Eden内存不够了之后，移动部分到老年代,然后新的依然在eden创建 (空间分配担保)
```

