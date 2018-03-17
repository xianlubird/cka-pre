## Understand how to monitor all cluster components
集群的组件主要有api-server, scheduler, controller-manger，目前对于监控，每台机器都会有一个cadvisor，它会收集每台机器上容器使用的内存，CPU和网络磁盘等情况，然后通过默认部署的Heapster组件汇集传到API Server。

- 这篇文章对于监控组件，以及监控组件之间的关系描述的非常清楚，建议看一看，[链接](https://sysdig.com/blog/monitoring-kubernetes-with-sysdig-cloud/)
- 另外，现在对于k8s集群的监控，prometheus-operator是一个非常流行的开源方案，具体信息可以查看[这里](https://github.com/coreos/prometheus-operator)
- k8s也提供了对于基础组件的Metrics获取的方式，可以直接`http://localhost:10252/metrics` 就能获取基础管控组件的工作详细。参考文档，[链接](https://kubernetes.io/docs/concepts/cluster-administration/controller-metrics/) 