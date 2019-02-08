KCP C#版。目标框架dotnetstandard2.0。

开箱即用。也可以使用Nuget 搜索KCP。

链接：

c: skywind3000 [KCP](https://github.com/skywind3000/kcp)  
go: xtaci [kcp-go](https://github.com/xtaci/kcp-go)  

用法：

请参考C版本文档。

说明：

- 内部使用了unsafe代码和非托管内存，所以kcpsegment运行时不会alloc，不会对gc造成压力。

- 对于output回调和TryRecv函数。使用RentBuffer回调，从外部分配内存。请参考IMemoryOwner用法。
- 支持Span<byte>



相对C版的一些变化：

| 差异变化       | C版            | C#版                                                  |
| -------------- | -------------- | ----------------------------------------------------- |
| 数据结构       |                |                                                       |
| acklist        | 数组           | ConcurrentQueue                                       |
| snd_queue      | 双向链表       | ConcurrentQueue                                       |
| snd_buf        | 双向链表       | LinkedList                                            |
| rcv_buf        | 双向链表       | LinkedList                                            |
| rcv_queue      | 双向链表       | List                                                  |
| -------------- | -------------- | --------------                                        |
| 回调函数       |                | 增加了RentBuffer回调，当KCP需要时可以从外部申请内存。 |
| 多线程         |                | 增加了线程安全。                                      |
| 流模式         |                | 由于数据结构变动，移除了流模式。                      |
| API变动        |                | 增加TryRecv函数，当可以Recv时只peeksize一次。         |