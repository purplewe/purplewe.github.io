---
title: 对tomcat的一些理解.md
tags:
---

### 架构分析

### 源码分析

以tomcat８源码在windows环境下为例，首先是批处理文件startup.bat，主要是设置CATALINA_HOME并且查找是否有catalina.bat文件，如果有就执行。

```bat
set "CATALINA_HOME=%CURRENT_DIR%"
set "EXECUTABLE=%CATALINA_HOME%\bin\catalina.bat"
call "%EXECUTABLE%" start %CMD_LINE_ARGS% 

```

然后是执行catalina.bat文件，首先校验CATALINA_HOME是否存在，并设置CATALINA_BASE的值

#### CATALINA_HOME与CATALINA_BASE表示什么，他们有什么区别。

CATALINA_HOME在startup.bat进行设置，CATALINA_BASE在catalina.bat中进行设置。
CATALINA_HOME可以看作是指向tomcat的安装目录，CATALINA_BASE可以看作是指向tomcat的工作目录。当一台服务器下运行多个tomcat实例时，而我们又不想安装多个tomcat时，就可以创建多个tomcat工作目录，工作目录包含conf、logs、temp、webapps、work和shared目录，每一个tomcat实例的CATALINA_HOME指向安装目录，CATALINA_BASE指向当前实例的工作目录。

### 问题

#### 为什么tomcat启动脚本要分startup和catalina，直接写在一个文件中启动不行吗？