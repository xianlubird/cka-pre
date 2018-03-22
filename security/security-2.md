## Understand Kubernets security primitives
对一个集群增加安全性，主要从控制所有针对API的操作和ETCD数据来进行，官方文档进行了很详细的介绍，[Securing a Cluster](https://kubernetes.io/docs/tasks/administer-cluster/securing-a-cluster/)。

另外，对于Pod的安全性，1.9版本也加入了新的特性[Pod Security Policies](https://kubernetes.io/docs/concepts/policy/pod-security-policy/)