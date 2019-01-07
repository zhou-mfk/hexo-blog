---
title: Rsyslog(二) 配置介绍
date: 2019-01-06 20:34:40
categories: Linux
tags: ["Rsyslog", "运维", "日志"]

---

#### Rsyslog 介绍

* Rsyslog 版本说明 ，此文的配置主要是在v8.19.0以上的配置，低版本的配置就不再写了，有需要的朋友可以到网上去找找。
* 来看一下官方网站的提供的一张图片，具体的介绍官方网站说的比较详细
  ![rsyslog](rsyslog-config/rsyslog-features-imagemap.png)



* 我是在CentOS下安装的Rsyslog v 8.19.0版本，也可以安装更高的版本，CentOS下默认已安装的v5.18的版本，此版本比较低，可以升级到更高的版本. 安装方法见上篇文章
* 
#### Rsyslog 配置介绍
* 配置文件
  主配置文件 /etc/rsyslog.conf
  以下是我精简后的配置
``` bash
# 工作目录
$WorkDirectory /var/lib/rsyslog
# 加载的模块，也可以放在其他配置文件中但是只能引用一次，如果多个配置中都进行了module，会有警告出现。
# 以下两个是默认加载的模块
module(load="imuxsock")
module(load="imklog")
# 用于读取日志文件的模块
module(load="imfile")
# 定义的模板
$template IpTemplate,"%HOSTNAME% %syslogtag% %msg:::sp-if-no-1st-sp%%msg:::drop-last-lf%"

$ActionFileDefaultTemplate RSYSLOG_TraditionalFileFormat
# 其他配置文件目录，在启动后需要加载
$IncludeConfig /etc/rsyslog.d/*.conf
# 以下的配置是指定的日志内容不写入/var/log/messages文件中
*.info;mail.none;authpriv.none;cron.none            /var/log/messages
authpriv.*                                              /var/log/secure
mail.*                                                  /var/log/maillog
cron.*                                                  /var/log/cron
*.emerg                                                :omusrmsg:*
uucp,news.crit                                          /var/log/spooler
local7.*                                                /var/log/boot.log
```
/etc/rsyslog.d 这个目录可以存放其他配置文件

#### Rsyslog 常用模块
Rsyslog常用到模块有输入类的输出类
* imfile # 输入类 读取文件的需要，手动指定 Path绝对路径
* omfile # 输出到文件
* omfwd # 转发模块
* 输出类模块见以下链接：[Rsylog输出模块](https://www.rsyslog.com/doc/v8-stable/configuration/modules/idx_output.html)

#### Rsyslog TCP OR UDP
Rsyslog 使用TCP还是UDP ？

一般来说选择TCP都是OK的，除非忍受部分丢失，在意影响性能，可以改用UDP。
但是注意：如果你的消息每行大小超过了4k，只能用TCP。这是因为UDP栈大小限制的

经过测试在udp转发日志时在每秒3000条日志左右就会有丢失，所以在用于收集高并发nginx日志时最好是使用TCP

#### Rsyslog 模板
模板类型有以下几种:
List, String等
这里只说String(因为只用到了String类型的，也用有json格式的，不过写成String类的，下面有介绍)
旧格式如下：
``` bash
$template IpTemplate,"%HOSTNAME% %syslogtag% %msg:::sp-if-no-1st-sp%%msg:::drop-last-lf%"

上面的变量说明
$templae 定义模板用的
IpTemplate 是模板的名字
%HOSTNAME% 这个是主机名
%syslogtag% 日志的标签,很有用的，如果用于收集nginx日志时就可以把这个标签用于标记不应用的日志
%msg:::sp-if-no-1st-sp%%msg:::drop-last-lf% 这个就是用于解析日志内容的了。
#
也可以在模板中写入主机的ip地址，但是模板不会自动获得ip地址
有一个%fromhost-ip% 这个只是在接收端为rsyslog服务时才会有用，如果服务端换成其他类型的如Graylog时却解析不出ip地址信息
```

#### 然后
此篇文章就写了关于rsyslog的配置，但是如何具体使用会在下一篇文章介绍
