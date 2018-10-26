# 实验： pin add 不同大小的文件

## 实验设施 

- 使用 ipfs-cluster 在华南区配置两个集群。集群 A 包含 south01, south02, south03 这三台机器。集群 B 包含 south04 这一台机器。
- 四台机器的配置皆为： 2核 4G内存  5M带宽
- 集群A 中配置 pin add 所获得的副本数为1，借此观察集群是否做了存储负载均衡

## 实验步骤

- Step 1. 在 south04 上使用 [gowrite 工具](https://github.com/csxuejin/gowrite) 创建不同大小的文件
- Step 2. 在 south04 上使用 [ipfstools 工具](https://github.com/csxuejin/ipfstools) 进行 ipfs cluster add 操作，并且生成记录文件哈希的文件 filehashes.
- Step 3. 将 filehashes 文件拷贝到 south01 上
- Step 4. 在 south01 上使用 [ipfstools 工具](https://github.com/csxuejin/ipfstools) 进行 ipfs cluster pin add 操作


### 实验结果

|  文件大小 | 试验次数 | 并发数 | 成功率 |
| :-: | :-: | :-: | :-: |
| 4M | 5 |  1 | 100% |
| 500M | 5 |  1 | 100% |
| 2G | 3 |  1 | 100% |
| 10G | 3 |  1 | 100% |


### 实验结论

- IPFS Cluster 不同集群之间可以进行上述大小文件的 pin add 操作
- 实验过程中发现 IPFS Cluster 具有存储负载均衡功能，能够自动将文件存储到不同机器上