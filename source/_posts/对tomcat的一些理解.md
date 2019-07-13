---
title: 对tomcat的一些理解.md
date: 2019-06-30 17:20:56
tags:
---


### 架构分析

### 源码分析

以tomcat８源码为例，首先是批处理文件startup.bat，主要是设置CATALINA_HOME并且查找是否有catalina.bat文件，如果有就执行。

```bat
set "CATALINA_HOME=%CURRENT_DIR%"
set "EXECUTABLE=%CATALINA_HOME%\bin\catalina.bat"
call "%EXECUTABLE%" start %CMD_LINE_ARGS% 

```

#### startup.bat
