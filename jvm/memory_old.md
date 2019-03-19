# 4.5.3：大对象直接分配到老年代

### 1: 代码案例

```java
public class MemoryOld {
    public static void main(String[] args) {
        byte[] bytes = new byte[4 * 1024 * 1024];
    }
}
```



### 2: jvm 运行参数

```
-verbose:gc -XX:+PrintGCDetails -XX:PretenureSizeThreshold=3M -Xms20M -Xmx20M
```



### 3: 运行结果

```
Heap
 PSYoungGen      total 6144K, used 1899K [0x00000007bf980000, 0x00000007c0000000, 0x00000007c0000000)
  eden space 5632K, 33% used [0x00000007bf980000,0x00000007bfb5af68,0x00000007bff00000)
  from space 512K, 0% used [0x00000007bff80000,0x00000007bff80000,0x00000007c0000000)
  to   space 512K, 0% used [0x00000007bff00000,0x00000007bff00000,0x00000007bff80000)
 ParOldGen       total 13824K, used 4096K [0x00000007bec00000, 0x00000007bf980000, 0x00000007bf980000)
  object space 13824K, 29% used [0x00000007bec00000,0x00000007bf000010,0x00000007bf980000)
 Metaspace       used 3165K, capacity 4496K, committed 4864K, reserved 1056768K
  class space    used 347K, capacity 388K, committed 512K, reserved 1048576K
```





### 4: 分析

```
如 jvm 运行参数可见,大于 3M 的会直接被放到老年代里面
从运行结果看，13824K 使用了29%,恰恰是4M ,符合最初的预期
```

