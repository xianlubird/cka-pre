## Manage cluster component logs
 对于集群而言，有两种部署方式。一种通过pod部署的方式，可以简单的通过`kubelet logs `命令获取具体的pod日志内容。另外一种属于进程方式部署，这种方式的日志就应该直接在磁盘目录或者指定的工具获取。

 官网详细的解说了log的存储方式以及使用方法，[链接](https://kubernetes.io/docs/concepts/cluster-administration/logging/)