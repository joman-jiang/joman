---
title: Centos下安装Docker
tags:
  - docker
categories: Docker
keywords: 'docker'
description: docker的安装
cover: /img/docker/docker.jpg
date: 2024-02-23 15:20:11
comments: false
---
### 一、背景
记录脚本，直接安装docker工具
### 二、脚本
```bash
## 防火墙
systemctl stop firewalld.service
#开机禁止启动
systemctl disable firewalld.service
# 所有节点关闭 SELinux，如logrotate对Nginx需要.查看getenforce
setenforce 0
sed -i --follow-symlinks 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/sysconfig/selinux

## 时间调整
rm -rf /etc/localtime
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

## 安装目录设置，只要是服务器资源在home下
mkdir -p /home/docker
ln -s /home/docker /var/lib

## 安装
yum install -y yum-utils device-mapper-persistent-data lvm2
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
yum install -y docker-ce docker-ce-cli containerd.io docker-compose
cat >/etc/docker/daemon.json<<EOF
{
	"registry-mirrors": ["https://i7yga2lt.mirror.aliyuncs.com"],
	"exec-opts": ["native.cgroupdriver=systemd"],
	"log-driver": "json-file",
	"log-opts": {
		"max-size": "50m",
		"max-file": "5"
	}
}
EOF
systemctl start docker
systemctl enable docker

```
