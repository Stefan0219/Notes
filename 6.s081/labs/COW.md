1. 内核中的代码必须考虑周到，在进行每次函数调用时，**必须检查返回值**
2. ![[Pasted image 20221106101609.png]]
   进行地址翻译时，通过VPN找到对应的PPN，最底层的PPN与VA的低位偏移结合，生成物理地址。
   **一个最底层的PTE** 对应4K个物理地址的缩写
   在进行引用计数时，释放的时机要考虑好，要在释放的最底层考虑。（kfree）以后也应注意。
   COW时，应检查该地址是否有效（是否为该进程持有的资源）