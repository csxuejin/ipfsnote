# 实验：自定义某文件副本数

ipfs-cluster-ctl 工具为我们提供了可以自定义某个文件副本数的功能。命令如下所示，该命令将会忽略全局设定的 `replication_factor_min` 和 `replication_factor_max` 参数，而设定该 CID 所对应内容的副本数为 value。

```
ipfs-cluster-ctl pin add -r <value> <CID>
```


## 实验设施 

- 使用 ipfs-cluster 在华南区配置两个集群。集群 A 包含 south01, south02, south03 这三台机器。集群 B 包含 south04 这一台机器。
- 四台机器的配置皆为： 2核 4G内存  5M带宽。
- 集群 A 中设置 `replication_factor_min` 和 `replication_factor_max` 皆为 -1，代表默认情况下所有节点都要存一份副本。

### 实验步骤

- （1）在 south04 生成大小约为 200M 的内容随机的文件 `XzpLGIehps`
- （2）在 south04 中执行 `ipfs-cluster-ctl add XzpLGIehps`，生成该文件对应的哈希 `QmcAZ3SM4UXkAE3rCLiyx8Ui6d5Nrjis5h8siZCarUYQ3i`。
- （3）在 south01 中执行 `ipfs-cluster-ctl pin add -r 1 QmcAZ3SM4UXkAE3rCLiyx8Ui6d5Nrjis5h8siZCarUYQ3i`
- （4）观察 south01, south02, south03 在步骤（3）执行前后 ~/.ipfs/blocks 文件夹的大小变化如下：


|  | south01 | south02 | south03 | 
| :-: | :-: | :-: | :-: |
| pin add 之前 | 140K | 516M | 20M |
| pin add 之后 | 140K | 516M | 223M |

结论：集群中只存了一份副本。说明 `ipfs-cluster-ctl pin add -r <value> <CID>`  这一命令可以有效覆盖全局设定的 `replication_factor_min` 和 `replication_factor_max` 参数。
