# IPFS Cluster 

> IPFS cluster is a stand-alone application and a cli client to allocate, replicate and track Pins across a cluster of IPFS daemons.

在这一部分中，我们将会介绍：

- IPFS Cluster 常用命令
- IPFS Cluster 集群启动方式
- IPFS Cluster 相关实验

## IPFS 常用命令

[点击此文](commands.md)

## IPFS Cluster 集群启动方式

- Starting multiple peers with a fixed peerset
- Starting a single peer and bootstrapping the rest to it

前者指的是，预设好集群节点信息，然后依次启动各个节点。具体参见[这篇文档](peerset.md)。

后者指的是，已经存在一个集群，然后有新的节点需要加入该集群，使用 bootstrap 方法，参见[这篇文档](bootstrap.md)。

