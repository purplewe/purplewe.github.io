---
title: 对tomcat的学习
date: 2019-06-30 17:20:56
tags:
---

### 1. tomcat架构图

![Please Wait](Architecture.png)

tomcat的顶层架构如上图所示，可以看到有server、service、connerctor、container四个组件，那么这四个组件又有什么作用呢？

**server：**

Server向客户端提供服务，并且服务器可以start或stop。

缺点：如果需要该服务器支持多种网络协议，就很难扩展，因此改进如下：

**connector和container**

将容器拆分成Connector以及Container，Connector用于和客户端连接，可以扩展支持多种协议，建立连接之后将socket交给Container处理。

**Service**

通过service统一connector和container的映射关系

**Host**

在一个主机下支持多种域名服务

**Context**

可以部署多个应用

```
connector支持的运行模式？

- BIO：Java原生IO技术，同步阻塞IO

- NIO：Jdk1.4，非阻塞IO

- APR

支持http、https、AJP等协议

AJP协议适用于web容器和web容器的连接
```

### 2. tomcat目录结构
```
 /bin：脚本文件目录。
 /common/lib：存放所有web项目都可以访问的公共jar包（使用Common类加载器加载）。
 /conf：存放配置文件，最重要的是server.xml。
 /logs：存放日志文件。
 /server/webapps：来管理Tomcat-web服务用的。仅对TOMCAT可见，对所有的WEB APP都不可见（使用Catalina类加载器加载）。
 /shared/lib：仅对所有WEB APP可见，对TOMCAT不可见（使用Shared类加载器加载）。
 /temp：Tomcat运行时候存放临时文件用的。
 /webapps：web应用发布目录。
 /work：Tomcat把各种由jsp生成的servlet文件放在这个目录下。删除后，启动时会自动创建。
```

### 3. tomcat优化？

**基础调优**

适当调整tomcat的最大线程数、最大空闲线程数、连接超时时间

开启http压缩，客户端请求静态文件时，服务端将静态文件压缩发送至客户端，再有客户端浏览器进行解压

**内存调优**

-Xmx

-Xms

-Xmn设置年轻代大小

-Xss设置栈大小

-XX:NewRatio=num设置年轻代与老年代的比例

-XX:SurvivorRatio=num设置survivor与eden区的比例

-XX:MaxPermSize=num(M)设置永久区的大小

-XX:MetaspaceSize=200m;-XX:MaxMetaspaceSize=256m;jdk1.8后使用hotspot虚拟机方法区的实现变为元空间


**垃圾回收策略调优**

搭配不同的垃圾回收器和垃圾回收算法


有空分析一下tomcat的启动脚本文件
```bat
set "CATALINA_HOME=%CURRENT_DIR%"
set "EXECUTABLE=%CATALINA_HOME%\bin\catalina.bat"
call "%EXECUTABLE%" start %CMD_LINE_ARGS% 

```