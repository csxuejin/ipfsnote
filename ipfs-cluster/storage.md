# 实验：ipfs-cluster 存储与回收

### 实验一 每个节点存储一个副本

- 准备两个集群：cluster-a(包含，epn-ipfs-011 和 epn-ipfs-012 两个节点), cluster-b (包含 epn-ipfs-014 一个节点)
- cluster-a 和 cluster-b 处于正常运行状态 
- ~/.ipfs-cluster/service.json 文件中 `replication_factor_min` 和 `replication_factor_max` 均为默认值 -1
- 在 cluster-b 中使用以下命令添加一个大文件

```
deploy@epn-ipfs-014:~$ ipfs --api /ip4/127.0.0.1/tcp/9095 add movie.tar
added QmQdnBv613cjdXppGjBBHUpeJE2myJiy3CUQSXPrQ2LLWf movie.tar
 557.51 MiB / 557.51 MiB [===============================================] 100.00%
```

- 在 cluster-a 中的 epn-ipfs-011 机器上进行 `pin add` 操作

```
deploy@epn-ipfs-011:~/.ipfs$ ipfs --api /ip4/127.0.0.1/tcp/9095 pin add QmQdnBv613cjdXppGjBBHUpeJE2myJiy3CUQSXPrQ2LLWf
pinned QmQdnBv613cjdXppGjBBHUpeJE2myJiy3CUQSXPrQ2LLWf recursively
```

上述 `pin add` 之后，对比 epn-ipfs-011 和 epn-ipfs-012 两台机器的 ~/.ipfs/blocks 文件夹变化如下表：

|  | epn-ipfs-011 | epn-ipfs-012 |
| :-: | :-: | :-: |
| pin add 之前 | 172K | 160K |
| pin add 之后 | 571M | 571M |

结论：集群中每个节点都存了一份文件副本。显然，这不是我们想要的，需要继续实验如何控制副本数。


### 实验二 删除副本，回收存储

- 建立在实验一的基础上
- 在 epn-ipfs-011 上运行 ipfs --api /ip4/127.0.0.1/tcp/9095 pin rm QmQdnBv613cjdXppGjBBHUpeJE2myJiy3CUQSXPrQ2LLWf

运行上述命令，实际上会让该集群中所有节点都执行了 pin rm 命令。验证：在任意一台上执行  `ipfs pin ls | grep QmQdnBv613cjdXppGjBBHUpeJE2myJiy3CUQSXPrQ2LLWf` 结果都为空。

- 在 epn-ipfs-011 上运行 ipfs --api /ip4/127.0.0.1/tcp/9095 repo gc

运行上述命令，发现 epn-ipfs-011 上的 ~/.ipfs/blocks 文件夹中的内容已经被顺利回收。但是 epn-ipfs-012 中的 ~/.ipfs/blocks 文件夹大小不变，仍然为 571M。

结论：回收操作只能立刻回收运行该命令的机器上的空间，但是无法立即在该集群上所有节点中对该内容占用的空间进行回收。如果要尽快回收所有节点的存储空间，可以有两种做法：
- （1）配置 ~/.ipfs/config 文件中的 gc 策略，加快回收速度  
- （2）设立一个中心，让拥有该内容的每个集群节点都执行一次 GC，这样可以立即回收对应存储空间。

### 实验三 控制副本数

- 在 epn-ipfs-014 中运行 add 操作

```
deploy@epn-ipfs-014:~$ ipfs --api /ip4/127.0.0.1/tcp/9095 add newmovie.tar
added QmVgGvMXxmf3aF9Dte7EFY8i3MmQmebFnv2ZcNg7sN3Px4 newmovie.tar
 557.58 MiB / 557.58 MiB [================================================] 100.00%
```

- 修改 ~/.ipfs-cluster/service.json 文件中的两个配置项： `replication_factor_min` 和 `replication_factor_max`。
    - replication_factor_min  代表存储该文件的集群最小节点数，可以理解为副本数下线，-1 代表全部。本次实验中设置为 1.
    - replication_factor_max  代表存储该文件的集群最大阶段属，可以理解为副本数上限，-1 代表全部。本次实验中设置为 1.

- 在 epn-ipfs-011 中执行 `pin add` 操作

```
deploy@epn-ipfs-014:~$ ipfs --api /ip4/127.0.0.1/tcp/9095 pin add QmVgGvMXxmf3aF9Dte7EFY8i3MmQmebFnv2ZcNg7sN3Px4
```

上述 `pin add` 之后，对比 epn-ipfs-011 和 epn-ipfs-012 两台机器的 ~/.ipfs/blocks 文件夹变化如下表：

|  | epn-ipfs-011 | epn-ipfs-012 |
| :-: | :-: | :-: |
| pin add 之前 | 3.7M | 3.7M |
| pin add 之后 | 571M | 3.7M |


结论：通过参数(replication_factor_min, replication_factor_max)可以控制副本数。本次实验中控制了副本数为 1.


### 参考文献

[Composite clusters](https://cluster.ipfs.io/documentation/composite-clusters/#a-cluster-of-clusters)


