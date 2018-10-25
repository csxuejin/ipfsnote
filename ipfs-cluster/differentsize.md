# 实验： pin add 不同大小的文件

## 实验设施 

- 两台华南区的机器（south01, south04），分别来自采用 ipfs-cluster 搭建的不同集群中。
- 两台机器的配置为： 2核 4G内存  5M带宽

## 实验步骤

- Step 1. 在 south04 上使用 [gowrite 工具](https://github.com/csxuejin/gowrite) 创建不同大小的文件
- Step 2. 在 south04 上使用 [ipfstools 工具](https://github.com/csxuejin/ipfstools) 进行 ipfs cluster add 操作，并且生成记录文件哈希的文件 filehashes.
- Step 3. 将 filehashes 文件拷贝到 south01 上
- Step 4. 在 south01 上使用 [ipfstools 工具](https://github.com/csxuejin/ipfstools) 进行 ipfs cluster pin add 操作


### 实验结论

|  文件大小 | 试验次数 | 并发数 | 成功率 |
| :-: | :-: | :-: | :-: |
| 4M | 3 |  1 | 100% |
| 100M | 3 |  1 | 100% |
| 500M | 3 |  1 | 100% |
| 1G | 3 |  1 | 100% |
| 10G | 3 |  1 | 100% |

