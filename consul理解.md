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
