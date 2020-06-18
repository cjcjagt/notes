# notes
# SMP和MPP架构对比
- SMP(Symmetric Multi-Processor)又称对称多处理器结构

SMP系统内有许多紧耦合多处理器，在这样的系统中，所有的CPU共享全部资源，如总线，内存和I/O系统等;
所谓对称多处理器结构，是指服务器中多个 CPU 对称工作，无主次或从属关系。各 CPU 共享相同的物理内存，每个 CPU 访问内存中的任何地址所需时间是相同的，因此 SMP 也被称为一致存储器访问结构 (UMA ： Uniform Memory Access) 。对 SMP 服务器进行扩展的方式包括增加内存、使用更快的 CPU 、增加 CPU 、扩充 I/O( 槽口数与总线数 ) 以及添加更多的外部设备 ( 通常是磁盘存储 ) 。
主要特征是共享，系统中所有资源 (CPU 、内存、 I/O 等 ) 都是共享的。也正是由于这种特征，导致了
SMP 服务器的主要问题，那就是它的扩展能力非常有限。对于 SMP 服务器而言，每一个共享的环节都可能造成 SMP 服务器扩展时的瓶颈，而最受限制的则是内存。由于每个 CPU 必须通过相同的内存总线访问相同的内存资源，因此随着 CPU 数量的增加，内存访问冲突将迅速增加，最终会造成 CPU 资源的浪费，使 CPU 性能的有效性大大降低。实验证明， SMP 服务器 CPU 利用率最好的情况是 2 至 4 个 CPU 。
- MPP(Massive Parallel Processing)海量并行处理结构

由多个 SMP 服务器通过一定的节点互联网络进行连接，协同工作，完成相同的任务，从用户的角度来看是一个服务器系统。其基本特征是由多个 SMP 服务器 ( 每个 SMP 服务器称节点 ) 通过节点互联网络连接而成，每个节点只访问自己的本地资源 ( 内存、存储等 ) ，是一种完全无共享 (Share Nothing) 结构，因而扩展能力最好，理论上其扩展无限制，目前的技术可实现 512 个节点互联，数千个 CPU 。但节点互联网仅供 MPP 服务器内部使用，对用户而言是透明的。


# 行存储与列存储
行存储的写入是一次性完成，消耗的时间比列存储少，并且能够保证数据的完整性，缺点是数据读取过程中会产生冗余数据，如果只有少量数据，此影响可以忽略；数量大可能会影响到数据的处理效率。列存储在写入效率、保证数据完整性上都不如行存储，它的优势是在读取过程，不会产生冗余数据，这对数据完整性要求不高的大数据处理领域，比如互联网，犹为重要。

什么时候应该使用行式存储？什么时候应该使用列式存储呢？
如果你大部分时间都是关注整张表的内容，而不是单独某几列，并且所关注的内容是不需要通过任何聚集运算的，那么推荐使用行式存储。原因是重构每一行数据（即解压缩过程）对于HANA来说，是一个不小的负担。
列式存储的话，比如你比较关注的都是某几列的内容，或者有频繁聚集需要的，通过聚集之后进行数据分析的表。



## 基础
https://blog.csdn.net/Ppikaqiu/article/details/106796242?utm_medium=distribute.pc_category.none-task-blog-hot-1.nonecase&depth_1-utm_source=distribute.pc_category.none-task-blog-hot-1.nonecase&request_id=

https://blog.csdn.net/weixin_44519467/article/details/106746310?utm_medium=distribute.pc_category.none-task-blog-hot-8.nonecase&depth_1-utm_source=distribute.pc_category.none-task-blog-hot-8.nonecase&request_id=

http://www.apiref.com/spring5/overview-summary.html


spring: https://blog.csdn.net/supingemail/article/details/85988220
https://blog.csdn.net/weixin_38361347/category_9281139_2.html


面试题总结：https://blog.csdn.net/hxpjava1/category_9268106.html
https://blog.csdn.net/weixin_38361347/category_9281139.html


mysql：https://blog.csdn.net/mifffy_java/article/details/106767628
https://blog.csdn.net/qq_39088066/article/details/101438677

## dubbo

Dubbo是一个分布式服务框架，致力于提供高性能和透明化的RPC远程服务调用方案，以及SOA服务治理方案。
其核心部分包含:
1. 远程通讯: 提供对多种基于长连接的NIO框架抽象封装，包括多种线程模型，序列化，以及“请求-响应”模式的信息交换方式。
2. 集群容错: 提供基于接口方法的透明远程过程调用，包括多协议支持，以及软负载均衡，失败容错，地址路由，动态配置等集群支持。
3. 自动发现: 基于注册中心目录服务，使服务消费方能动态的查找服务提供方，使地址透明，使服务提供方可以平滑增加或减少机器。

2. Dubbo能做什么？
1.透明化的远程方法调用，就像调用本地方法一样调用远程方法，只需简单配置，没有任何API侵入。      
2.软负载均衡及容错机制，可在内网替代F5等硬件负载均衡器，降低成本，减少单点。
3. 服务自动注册与发现，不再需要写死服务提供方地址，注册中心基于接口名查询服务提供者的IP地址，并且能够平滑添加或删除服务提供者。

https://blog.csdn.net/houshaolin/article/details/76408399


## SQL
SQL 优化

1.尽量全值匹配
2.最佳左前缀法则
3.不在索引列上做任何操作
4.范围条件放最后
5.覆盖索引尽量用
6.不等于要慎用
7.Null/Not 有影响
8.Like 查询要当心
9.字符类型加引号
10.OR 改 UNION 效率高


MYSQL实现读写分离 ：https://www.jianshu.com/p/bc45c8bccf3c

