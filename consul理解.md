![consul架构图](https://www.consul.io/assets/images/consul-arch-420ce04a.png "consul架构图")

## Agent
我觉得应该叫node，类似elasticsearch中node的概念，是组成consul集群的成员。即consul cluster的所有几点必须启用一个agent，agent有两种运行模式：client、server

## Client 
Agent的一种运行模式；对比es中client节点（不存储数据，将请求转发值master）
+ 无状态
+ 将（rpc）请求转发给server节点
+ 参与LAN gossip pool
+ 资源开销小，宽带占用低

## Server
Agemt的另一种运行模式；对比es中的master节点（即有被选中为leader的权利）
+ 维护集群状态
+ leader选举（作为集群法人）
+ 监控检查
+ 响应rpc请求
+ 和其他数据中心交换WAN gossip
+ 转发query到远程数据中心
+ 保持数据一致性
+ 需3个已上server组成集群

## DataCenter
个人认为是一个定义，就是对agent进行分组。我们一般依据以下条件第一一个数据中心：
+ 私有网路环境
+ 低延迟
+ 高宽带

## Consensus
leader选举

## Gossip
gossip协议，用于
+ membership（成员关系维护，成员沟通）
+ failure detection（故障检测）
+ event broadcast（时间广播）
+ 使用使用udp协议

## LAN gossip
一个LAN或者datacenter内地gossip pool

## WAN gossip
不同datacenter的server和通过公网或者WAN通讯的gossip pool

## Others
+ 一个数据中心应该有3-5个server
+ client数量无限制

+ 一个数据中心会维护一个LAN gossip pool，这样做的好处
  - 对于clients无需配置server信息，可以自动发现client
  - 故障检测的工作是分布式的，为故障检测提供了更高的可用性和更好的灵活性
  - 可以作为消息层来进行一些重要的事件通知，比如leader选举
 
+ 一个数据中的所有server一起工作选举出一个leader
  - leader负责处理所有的查询和事物
  - 非leader的server收到的rpc请求将转发给leader进行处理

+ 不同数据中的server会维护一个 WAN gossip pool
  - 多个数据中心的互相发现
  - 可以将请求转至其他数据中心

+ 低耦合，故障检测，连接缓存和复用，跨数据中心请求更快更可靠

+ 一般情况下，不同数据中心的数据不会同步；但是有些情况下可以复制有限的数据子集



