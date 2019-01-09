---
title: elasticsearch使用及配置
date: 2019-01-07 20:25:27
categories: ELK
tags: ["elastic", "ELK", "elasticsearch"]
---

# ElasticSearch 使用过程相关方法和配置

## 磁盘相关

设置elasticsearch集群磁盘空间使用百分比

在elasticsearch 5.x以上的版本最好使用百分比来调整集群磁盘空间，默认85%时就不会有数据写入。设置方法如下:

```shell
curl -X PUT http://your_elastic_server:9200/_cluster/settings -d \
'{
	"transient": {
		"cluster.routing.allocation.disk.watermark.low": "90",
		"cluster.routing.allocation.disk.warmark.high": "95%",
		"cluster.info.update.interval": "1m" 
		}
}'
```

## 集群中节点上下线

* 下线

  当我们需要对集群里的节点下线时，当如何操作呢？ 比如我们需要调整一台主机的内存时。这个时候就需要把这个节点下线掉。如何在保障数据的同时下线主机？操作如下:

  ```shell
  curl -X PUT 127.0.0.1:9200/_cluster/settings -d '{"transient: {"cluster.routing.allocation.exclude._ip":"x.x.x.x"}}'
  ```

  之后数据会看到数据从这个节点上迁移到其他主机上, 等到数据移除完后就可以所这台主机的elasticsearch服务停掉了。

  也可以同时下线多台主机

  ```shell
  curl -X PUT 127.0.0.1:9200/_cluster/settings -d '{"transient: {"cluster.routing.allocation.exclude._ip":"x.x.x.x, y.y.y.y"}}'
  ```

* 上线

  当主机调整好之后就需要上线, 上线时只需要把cluster.routing.allocation.exclude._ip的值为空即可。

  ```shell
  curl -X PUT 127.0.0.1:9200/_cluster/settings -d '{"transient: {"cluster.routing.allocation.exclude._ip":""}}'
  ```

  然后就可以看到数据又从其他节点上同步过来了。

  下面有对使用的cluster.routing.allocation.exclude._ip 这个集群属性的相关说明 

## 集群级别的 include/require/exclude 三种filter影响集群级的shard分配过程

```shell
cluster.routing.allocation.include.{attribute}: {value} 
cluster.routing.allocation.require.{attribute}: {value} 
cluster.routing.allocation.exclude.{attribute}: {value} 
```

其中{attribute} 是节点的属性, 即在es配置文件中以 node. 开头的属性(除了 node.name , node.client 和 node.data 这三个); 其中有4个属性比较特殊, 分别是:

```shell
_ip: 表示节点的ip
_host: 表示节点的hostname
_name(或name): 表示节点的名称, 即在Transwarp ES配置文件中的 node.name
_id: 表示节点的id; 节点id是es内部分配的
```

{value} 是属性值, 多个值之间用逗号(,)分割; 每个值支持通配符( a* , *a , *a* , a*b 这几种)

{attribute} 和 {value} 构成了一个过滤条件。 

include/require/exclude 的含义分别是: 

```shell
include : 节点的 {attribute} 值包含指定的至少一个value, value中的各个值是OR关系。 
require : 节点的 {attribute} 值包含指定的所有value, value中的各个值是AND关系。 
exclude : 节点的 {attribute} 值不包含指定的所有value。 
```

每种filter可以指定多个 {attribute} , 多个 {attribute} 之间是AND关系。

## 使用cerebro来管理elasticsearch集群

在elastic 5.0之后 kopf插件就算是不能再用了。还好找到一个好的开源的工具cerebro,安装及配置非简单, 功能与kopf一样, 与elastic偶合度低。github 地址: [Cerebro](https://github.com/lmenezes/cerebro)



