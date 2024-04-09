# kubernetes

master上运行着如下关键进程：

* Kubernetes API Server（kube-apiserver）：提供了HTTP Rest接口的关键服务进程，是Kubernetes里所有资源的增、删、改、查等操作的唯一入口，也是集群控制的入口进程。
* Kubernetes Controller Manager（kube-controller-manager）：Kubernetes里所有资源对象的自动化控制中心，可以将其理解为资源对象的“大总管”。
* Kubernetes Scheduler（kube-scheduler）：负责资源调度（Pod调度）的进程，相当于公交公司的“调度室”。

另外，在Master上通常还需要部署etcd服务，因为Kubernetes里的所有资源对象的数据都被保存在etcd中。



node 负载节点

* kubelet：负责Pod对应的容器的创建、启停等任务，同时与Master密切协作，实现集群管理的基本功能。
* kube-proxy：实现Kubernetes Service的通信与负载均衡机制的重要组件。
* Docker Engine（docker）：Docker引擎，负责本机的容器创建和管理工作。



可以通过kubectl describe pod xxxx来查看pod Event事件排查问题



◎ 版本标签："release" : "stable"、"release" : "canary"。

◎ 环境标签："environment":"dev"、"environment":"qa"、"environment":"production"。

◎ 架构标签："tier" : "frontend"、"tier" : "backend"、"tier" : "middleware"。

◎ 分区标签："partition" : "customerA"、"partition" : "customerB"。

◎ 质量管控标签："track" : "daily"、"track" : "weekly"。

