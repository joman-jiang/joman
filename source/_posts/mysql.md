---
title: Mysql分表方案
tags:
  - mysql
categories: Docs
keywords: 'mysql、分表'
description: Mysql分表方案
cover: /img/00.jpg
date: 2022-02-23 15:20:11
comments: false
---
### 一、背景
不断增长的数据处理需求和Mysql性能下降的矛盾，导致需求对Mysql数据库进行分库分表
### 二、术语
分库分表、ShardingSphere、垂直拆分、水平拆分
### 三、解决思路

- 数据分库分表  
服务集成ShardingSphere-jdbc，基础curd业务操作
- ES解决分页查询  
服务器集成Elasticsearch，主业务表分页查询
- 业务操作带有sharding key  
合理的业务分库字段是分表效率的保证，需要保证数据的均匀散落、查询可以带有分表标识

- mysql 和 elasticsearch数据同步方案  
参考 ==《数据接口处理负载异步计算方案》==


> 我们的业务水平采用的是单数据库服务器架构，即便存在多个数据库，但是总的io上限是不变的，所以不考虑采用分库分表策略。即采用分表策略

### 四、前置条件
- 稳定的业务功能  
- 健全的业务实现逻辑，内部数据查询粒度小，即减少大sql，采用多小sql实现  
- 主业务表数据达到500w+  
- 主业务表主键唯一性（分布式id、雪花id）  
- 存在可分表的业务字段或添加自定义规则的业务字段  
- 简化复杂业务逻辑，考虑是否可采取异步（采用第三方队列）计算  
- 移除程序内部的缓存，采用外部缓存redis或其他


### 五、shardingsphere集成
