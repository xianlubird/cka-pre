## Manage application logs
对于应用的日志查询，默认来说一般采用`kubectl logs`方式，对于pod crash等情况，也会留下尸体日志。但是对于节点挂点等极端情况，就会出现日志丢失的情况。所以对于日志的收集，需要采取一些三方的集成组件。

默认来说，k8s集成了`Stackdriver`，具体使用方式可以参考这个[文档](https://kubernetes.io/docs/tasks/debug-application-cluster/logging-stackdriver/)。

另外一种比较流行的日志集成工具就是ELK方案，不过这个方案比较重，比起前面的几种，实现起来基础组件占用的资源较多。但是功能比较强大。基本的使用可以参考这个[文档](https://kubernetes.io/docs/tasks/debug-application-cluster/logging-elasticsearch-kibana/)