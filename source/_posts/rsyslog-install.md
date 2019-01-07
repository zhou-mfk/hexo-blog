---
title: Rsyslog(一) 安装
date: 2019-01-06 20:32:40
tags: [Rsyslog, 运维, 日志]
---
#### Rsyslog（一） 安装
Rsysog v8 版本的安装方法（yum源）

* 配置yum源
``` bash
    cd /etc/yum.repos.d
    vim rsyslog.repo
    [rsyslog_v8]
    name=Adiscon CentOS-$releasever - local packages for $basearch
    baseurl=http://rpms.adiscon.com/v8-stable/epel-$releasever/$basearch
    enabled=1
    gpgcheck=0
    gpgkey=http://rpms.adiscon.com/RPM-GPG-KEY-Adiscon
    protect=1
```
* 卸载rsyslog 5.x 版本
  由于系统可能已安装了rsyslog 5.x版本的所以需要卸载
``` bash
yum remove rsyslog -y # 会卸载一些依赖包，注意
```
* 安装
```shell
yum install rsyslog -y
# 安装完成后
rpm -qa | grep rsyslog # 来查看安装的rsyslog包信息
```
