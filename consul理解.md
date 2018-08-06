![consul架构图](http://zhangyuyu.github.io/imgs/consul-arch.jpg "consul架构图")

## Agent
我觉得应该叫node，类似elasticsearch中node的概念

## Server 
node（Agent）的一个角色属性，server模式；对比es中master节点（master可以有多个，但是只能有一个leader）
+ 无状态
+ 将请求转发给server节点
+ 需要指定数据中心

## Client
node（Agemt）的一个角色属性，client模式，无状态，只是把请求转发给server节点；对比es中的client-node
+ 维护集群状态
+ leader选举
+ 监控检查
+ 3个已上组成集群
+ 保持数据一致性

## DataCenter
个人认为是个虚拟的概念，就是给server和client指定所属的组（datacenter），一个由server node组成的cluster也就是consul cluster
