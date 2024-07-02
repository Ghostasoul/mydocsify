[CSDN学习日志框架](https://www.cnblogs.com/haoworld/p/springboot-zheng-he-ri-zhi-kuang-jia.html)

[学习日志框架](https://blog.csdn.net/A_art_xiang/article/details/129702868)

---

## 一、 什么是日志门面？

---

### 1. 门面模式

门面模式（Facade Pattern），是GoF23种设计模式其中之一，也称之为外观模式，其核心为：外部与一个子系统的通信必须通过一个统一的外观对象进行，使得子系统更易于使用。

外观模式主要是体现了Java中的一种好的封装性。更简单的说，就是对外提供的接口要尽可能的简单。

### 2. 日志门面

常见的日志实现：JUL、log4j、logback、log4j2
常见的日志门面 ：JCL、slf4j
出现顺序 ：log4j -->JUL–>JCL–> slf4j --> logback --> log4j2

JUL、log4j、logback、log4j2这几种日志框架，每一种日志框架都有自己单独的API，要使用对应的框架就要使用其对应的API，这就大大的增加应用程序代码对于日志框架的耦合性。

为了解决这个问题，就是在日志框架和应用程序之间架设一个沟通的桥梁，对于应用程序来说，无论底层的日志框架如何变，都不需要有任何感知。只要门面服务做的足够好，随意换另外一个日志框架，应用程序不需要修改任意一行代码，就可以直接上线。

### 3. 市面上的日志框架

|日志门面|日志实现|
|-|-|
|JCL(Jakarta Commons Logging)、SLF4J（Simple Logging Facade for java）|Log4j、JUL（java.util.logging） Log4j2、Logback|








## 二、了解JCL

---

全称为Jakarta Commons Logging，是Apache提供的一个通用日志API。

用户可以自由选择第三方的日志组件作为具体实现，像log4j，或者jdk自带的jul， common-logging会通过动态查找的机制，在程序运行时自动找出真正使用的日志库。

当然，common-logging内部有一个Simple logger的简单实现，但是功能很弱。所以使用common-logging，通常都是配合着log4j以及其他日志框架来使用。

使用它的好处就是，代码依赖是common-logging而非log4j的API， 避免了和具体的日志API直接耦合，在有必要时，可以更改日志实现的第三方库。

JCL 有两个基本的抽象类：
Log：日志记录器
LogFactory：日志工厂（负责创建Log实例）

### 1. JCL组件结构

![](https://qcloudtest-1256407512.cos.ap-guangzhou.myqcloud.com/Pic%25E6%2588%25AA%25E5%25B1%258F2024-03-29%252015.46.59.png)

### 2. JCL案例

#### 1. JCL默认实现

#### 2. 导入log4j测试原有程序



## 三、 SLF4J简介

---



## 四、SLF4J基本使用

---



## 五、 SLF4J集成其他日志实现

---

