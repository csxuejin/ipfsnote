# 预设置集群节点方式

准备：

-  安装并运行 IPFS
-  根据操作系统下载预编译好的 ipfs ipfs-cluster-service 和 ipfs-cluster-ctl，本文以 ubuntu 为例
-  将可执行文件 ipfs-cluster-service 和 ipfs-cluster-ctl 拷贝到 /usr/local/bin 目录下，使其可以当做全局工具使用

以上是前置准备工作，每一个节点都需要做好这些前置步骤。接下来要进行集群配置。


## Step 0.  设置 CLUSTER_SECRET 环境变量

在机器 node0 上运行：

```
export CLUSTER_SECRET=$(od  -vN 32 -An -tx1 /dev/urandom | tr -d ' \n')
```

假设得到的 CLUSTER_SECRET 变量值为：

`9a420ec947512b8836d8eb46e1c56fdb746ab8a78015b9821e6b46b38344038f`

此后运行 `ipfs-cluster-service init`，那么生成的 ~/.ipfs-cluster/service.json 文件中 secret 值即为上述变量值。

接着在其余机器上运行如下命令，使 secret 保持一致：

```
export CLUSTER_SECRET=9a420ec947512b8836d8eb46e1c56fdb746ab8a78015b9821e6b46b38344038f

ipfs-cluster-service init
```

## Step 1. 配置 service.json 文件中 init_peerset 字段

该字段填写的内容是，每个节点的 service.json 文件中的 cluster.id 字段。示例如下： 

``` json
"init_peerset": [
    "QmeoUFMoWu1erV4XZv3VptTgfU1dHBAzA8zsZZpB9WmVkX",
    "QmbsNsnW5vFg9YXmDAfubz8PuAg4SNCKx8q6DUdbKTzEkd"
]
```

需要注意是
- init_peerset 需要包含所有节点的 id。所有节点的该字段值都相同。
- 这个 id 指的不是 ipfs id，而是 ipfs-cluster 的id，即 service.json 文件中的 cluster.id。

## Step 2. 配置 peerstore 文件

peerstore 所处的位置是 ~/.ipfs-cluster/peerstore 。该文件必须包含至少一个其他节点的 multiaddress。示例如下：

```
/ip4/192.168.0.14/tcp/9096/ipfs/QmeoUFMoWu1erV4XZv3VptTgfU1dHBAzA8zsZZpB9WmVkX
```

## Step 3. 启动

在每个节点中运行 `ipfs-cluster-service daemon&` ，注意如果原来 ~/.ipfs-cluster/raft 文件夹存在的话，先把该文件夹删除。

启动过后，稍等片刻，就可以查看集群节点信息：`ipfs-cluster-ctl peers ls`。结果示例如下：

```
QmbsNsnW5vFg9YXmDAfubz8PuAg4SNCKx8q6DUdbKTzEkd | epn-ipfs-011 | Sees 1 other peers
  > Addresses:
    - /ip4/127.0.0.1/tcp/9096/ipfs/QmbsNsnW5vFg9YXmDAfubz8PuAg4SNCKx8q6DUdbKTzEkd
    - /ip4/192.168.0.11/tcp/9096/ipfs/QmbsNsnW5vFg9YXmDAfubz8PuAg4SNCKx8q6DUdbKTzEkd
  > IPFS: QmPYLxvP9psgMJq2Dwj1GtDaKYbZrdEzPaLgu7jvHHvCNw
    - /ip4/127.0.0.1/tcp/4001/ipfs/QmPYLxvP9psgMJq2Dwj1GtDaKYbZrdEzPaLgu7jvHHvCNw
    - /ip4/157.255.51.177/tcp/4001/ipfs/QmPYLxvP9psgMJq2Dwj1GtDaKYbZrdEzPaLgu7jvHHvCNw
    - /ip4/192.168.0.11/tcp/4001/ipfs/QmPYLxvP9psgMJq2Dwj1GtDaKYbZrdEzPaLgu7jvHHvCNw
    - /ip6/::1/tcp/4001/ipfs/QmPYLxvP9psgMJq2Dwj1GtDaKYbZrdEzPaLgu7jvHHvCNw
QmeoUFMoWu1erV4XZv3VptTgfU1dHBAzA8zsZZpB9WmVkX | epn-ipfs-012 | Sees 1 other peers
  > Addresses:
    - /ip4/127.0.0.1/tcp/9096/ipfs/QmeoUFMoWu1erV4XZv3VptTgfU1dHBAzA8zsZZpB9WmVkX
    - /ip4/192.168.0.14/tcp/9096/ipfs/QmeoUFMoWu1erV4XZv3VptTgfU1dHBAzA8zsZZpB9WmVkX
  > IPFS: Qme6bkxMVu4cHcncuSy7kB4AvWGZpmNNaDeZXKqf2QGfj5
    - /ip4/127.0.0.1/tcp/4001/ipfs/Qme6bkxMVu4cHcncuSy7kB4AvWGZpmNNaDeZXKqf2QGfj5
    - /ip4/157.255.51.51/tcp/4001/ipfs/Qme6bkxMVu4cHcncuSy7kB4AvWGZpmNNaDeZXKqf2QGfj5
    - /ip4/192.168.0.14/tcp/4001/ipfs/Qme6bkxMVu4cHcncuSy7kB4AvWGZpmNNaDeZXKqf2QGfj5
    - /ip6/::1/tcp/4001/ipfs/Qme6bkxMVu4cHcncuSy7kB4AvWGZpmNNaDeZXKqf2QGfj5
```


## 参考文献

https://cluster.ipfs.io/documentation/configuration/#the-peerstore-file
https://cluster.ipfs.io/documentation/starting/#starting-multiple-peers-with-a-fixed-peerset