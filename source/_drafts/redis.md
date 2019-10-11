---
title: redis
tags:
---

开源的、基于键值对的存储系统

特性：

速度快
持久化
多种数据结构
多种编程语言
功能丰富
    发布订阅
    lua脚本
    事务
    pipline
简单
主从复制
高可用分布式

redis 应用场景

缓存系统
计数器
消息队列系统
排行榜
社交网络
实时系统

启动方式：

redis-server直接启动
动态参数启动
配置文件启动

redis常用配置：
daemonize守护进程方式启动
port
logfile
dir工作目录

redis API的使用：

通用命令（keys dbsize exists type expire type）
数据结构和内部编码（string hash list set zset）
单线程架构

1.为什么需要连接池
2.jedis直连、jedispool、jediscluster