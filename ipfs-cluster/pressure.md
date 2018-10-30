# 实验: IPFS Cluster 进行多并发 pin add 操作

## 实验设施 

- 使用 ipfs-cluster 在华南区配置两个集群。集群 A 包含 south01, south02, south03 这三台机器。集群 B 包含 south04 这一台机器。
- 四台机器的配置皆为： 2核 4G内存  5M带宽。
- 在 south04 上运行 Prometheus，在 south01、south02、south03 三台中运行 node_exporter。 Prometheus 每 5s 抓取一次 node_exporter 数据。

## 实验步骤

- Step 1. 在 south04 上使用 [gowrite 工具](https://github.com/csxuejin/gowrite) 创建不同大小的文件
- Step 2. 在 south04 上使用 [ipfstools 工具](https://github.com/csxuejin/ipfstools) 进行 ipfs cluster add 操作，并且生成记录文件哈希的文件 filehashes.
- Step 3. 将 filehashes 文件拷贝到 south01 上
- Step 4. 在 south01 上使用 [ipfstools 工具](https://github.com/csxuejin/ipfstools) 进行 ipfs cluster pin add 操作

> 在进行 pin add 操作的时候，控制并发数，借此观察并发数增加的情况下 CPU，内存和磁盘 IO 的变化。

### 实验结果

|  文件大小 | 文件数 | 并发数 | CPU 使用率 |
| :-: | :-: | :-: | :-: |
| 4M |  200 |  200 | ![cluster](imgs/4m200.png) |
| 4M | 500 | 500 | ![cluster](imgs/4m500.png) |
| 2G | 10 | 10 | ![cluster](imgs/2g10.png) | 
| 10G | 3 | 3 |  ![cluster](imgs/10g3.png) |
