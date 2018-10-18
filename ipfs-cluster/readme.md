# ipfs-cluster 入门


## 两种启动 ipfs 集群的方式

- Starting multiple peers with a fixed peerset
- Starting a single peer and bootstrapping the rest to it

前者指的是，预设好集群节点信息，然后依次启动各个节点。具体参见[这篇文档](peerset.md)。

后者指的是，已经存在一个集群，然后有新的节点需要加入该集群，使用 bootstrap 方法，参见[这篇文档](bootstrap.md)。

