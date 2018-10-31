# IPFS Cluster 概览

- IPFS Cluster 是 IPFS 集群的编排工具。
- 一个 IPFS 节点对应一个 IPFS Cluster 的 peer。
- IPFS Cluster peers 采用 libp2p 进行通信，与 IPFS 类似。
- 同一个 IPFS Cluster 的节点需要确保 Private Key 一致。

![overview](imgs/cluster-overview.png)


### 已有特性

- 多节点之间内容的复制，pin，add 操作。
- 从多节点中获取内容
- 采用基于 Raft 的共识
- 控制副本数的上限和下限
- 某个 pin 操作失败后，会自动重试
- 节点存储负载均衡
- 集群数据的导入和导出 （Pin-set exports and imports）
- 集群节点动态添加和移除

### 参考文献

https://cluster.ipfs.io/documentation/overview/
