<!--
 * @Author: tangdaoyong
 * @Date: 2021-01-26 18:00:30
 * @LastEditors: tangdaoyong
 * @LastEditTime: 2021-01-26 18:01:11
 * @Description: file content
-->
# k8s架构

## Master节点

Master节点指的是集群控制节点，管理和控制整个集群，基本上k8s的所有控制命令都发给它，它负责具体的执行过程。在Master上主要运行着：

Kubernetes Controller Manager（kube-controller-manager）：k8s中所有资源对象的自动化控制中心，维护管理集群的状态，比如故障检测，自动扩展，滚动更新等。
Kubernetes Scheduler（kube-scheduler）： 负责资源调度，按照预定的调度策略将Pod调度到相应的机器上。
etcd：保存整个集群的状态。

## Node节点

除了master以外的节点被称为Node或者Worker节点，可以在master中使用命令 kubectl get nodes查看集群中的node节点。每个Node都会被Master分配一些工作负载（Docker容器），当某个Node宕机时，该节点上的工作负载就会被Master自动转移到其它节点上。在Node上主要运行着：

kubelet：负责Pod对应的容器的创建、启停等任务，同时与Master密切协作，实现集群管理的基本功能
kube-proxy：实现service的通信与负载均衡
docker（Docker Engine）：Docker引擎，负责本机的容器创建和管理