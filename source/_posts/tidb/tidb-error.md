---
title: TiDB集群故障解决
date: 2023-07-04 13:31:29
tags:
categories: TiDB
---
##### 1、Tikv 删除sst文件无法启动解决
```shell
## 删除这个tikv节点再进行扩容
tiup cluster scale-in tidb-test -N 10.66.95.96:20160 --force
```
##### 2、恢复数据库完成出现错误
Error: failed to validate checksum: [BR:Restore:ErrRestoreChecksumMismatch]restore checksum mismatch
```shell
该错误没有实质影响，，主要是被恢复的库已存在，如果不想出现，可以先删除历史库，再进行恢复
```
##### 3、从S3恢复TIDB数据库出现错误
The difference between the request time and the server&#39;s time is too large.
```shell
1、检查S3服务时间是否正确
2、检查TIDB所有节点的时间是否正确
以上肯定存在时间不正确，修正系统时间和硬件时间即可
```

#### 参考
[minio部署在linux上, 上传图片报错解决](https://blog.csdn.net/qq_42616672/article/details/124307647)
[BR还原问题请教: BR:Restore:ErrRestoreChecksumMismatch](https://asktug.com/t/topic/303593)

