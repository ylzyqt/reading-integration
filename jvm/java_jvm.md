# 4.4：java虚拟机

### 1: java虚拟机

- sun classic vm

  ```
  已淘汰,第一款商用的java虚拟机
  纯解释器的方式来执行java代码
  ```

- Exact vm

  ```
  准确式内存管理
  编译器和解释器混合工作以及2级即时编译器
  只在solaris平台发布 (jdk1.2)
  ```

- Hotspot vm

  ```
  热点代码探测技术
  自2007年广泛运用
  ```

- kvm

  ```
  kilobyte 简单、轻量、高度可移植性
  手机平台运行
  ```

- jrockit

  ```
  bea
  专注服务端应用
  优势: 
    垃圾收集器
    Mission Control服务套件
  ```

- j9

  ```
  from IBM
  ```

- dalvik

  ```
  并不是java虚拟机,没有遵循java虚拟机规范
  Google公司自己设计用于Android平台的虚拟机
  ```

- Microsoft jvm

  ```
  java语言初期,微软为了兼容java applet,只能运行在windows平台下
  ```

- Azul vm liquid vm

  ```
  高性能的java虚拟机
  liquid vm不需要操作系统的支持，本身具备操作系统部分的功能
  ```

- taobao vm

  ```
  淘宝根据hotspot再定制的虚拟机产品
  只能使用因特尔的cpu
  ```