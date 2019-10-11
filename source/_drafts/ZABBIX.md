---
title: ZABBIX
tags:
---

## 监控系统主要涉及功能

- 数据收集
- 数据展示

- 警告策略
- 警告发送

- 事件管理
- 报表管理

## Zabbix 4.0

### Zabbix 简介与优点

> Zabbix 是一个开源的企业级的分布式监控方案。

监控各种机器的各种参数

灵活的通知机制

出色的报告和数据可视化

开源免费的

**问题** 

- [ ] 1. Zabbix 可以监控哪些设备或服务？

一台物理服务器，一个网络交换机，一个虚拟机或者一些应用。

- [ ] 2. Zabbix server 与被监控的设备或服务之间是如何通信的？<<==>>Zabbix 的数据收集方式有哪些？

### Zabbix 功能点（一些功能需要逐个理解）

1. 数据收集

2. 灵活的阈值定义

3. 可配置化的警告

4. 实时图表绘制

5. Web 监控功能

6. 丰富的可视化选项

7. 历史数据存储

8. 网络发现

9. 权限管理

10. Zabbix API

### Zabbix 组成结构

- Zabbix Server

是处理所有配置信息、统计信息和操作数据的核心组件。

- 数据库

- Web 界面

用于展示数据、提供操作数据的页面。

- Proxy 代理服务器

可以替Zabbix Server收集性能和可用性数据。

- agent 监控代理

部署在监控目标上，能够主动监控本地资源和应用程序，并将收集到的数据报告给Zabbix Server。

- 数据流

**问题**

- [ ] 1.0 Proxy 和 agent 傻傻分不清？

## Zabbix 进程

- Server

- Agent

passive

active

- Proxy

- Java Gateway

- Sender

- Get

## 部署 Zabbix

Zabbix 的部署方式有四种。

安装的内容包括：zabbix server、数据库、前端页面 

1.0 从分发包进行安装

特点：安装简单、可靠性强

- 安装步骤



2.0 下载最新的源码，并自行编译

3.0 从容器安装

4.0 下载虚拟应用


### 监控搜集


### 数据展示

## Zabbix server 安装

### 二进制文件安装

### 源码安装

### 容器安装

## Zabbix 前端介绍

## 主机监控部署

更换镜像源: wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-6.repo
安装拓展源: yum install epel-release

添加zabbix源: rpm -ivh http://repo.zabbix.com/zabbix/4.4/rhel/7/x86_64/zabbix-agent-4.4.0-1.el7.x86_64.rpm

yum search zabbix-agent
yum install -y zabbix-agent

systemctl start zabbix-agent.service
systemctl enable zabbix-agent.service

应用程序可用的物理内存值是Mem项的free值加上buffers和cached值之和，也就是说，这个free值是包括buffers和cached项大小的，对于应用程序来说，buffers/cached占有的内存是可用的，因为buffers/cached是为了提高文件读取的性能，当应用程序需要用到内存的时候，buffers/cached会很快地被回收，以供应用程序使用。

buffers是用来缓冲块设备做的，它只记录文件系统的元数据（metadata）以及 tracking in-flight pages，而cached是用来给文件做缓冲。更通俗一点说：buffers主要用来存放目录里面有什么内容，文件的属性以及权限等等。而cached直接用来记忆我们打开过的文件和程序。

## 主机监控部署

1. centos安装配置agent

## 多种查看数据的方式

### Latest data

### Graphs

### Screens

### Web 监控

zabbix-agent

rpm -ivh http://repo.zabbix.com/zabbix/4.4/rhel/7/x86_64/zabbix-agent-4.4.0-1.el7.x86_64.rpm

yum install -y zabbix-agent

# zabbix server端IP
Server=127.0.0.1

# zabbix server端IP
ServerActive=127.0.0.1

# zabbix客户端主机名
Hostname=Zabbix server