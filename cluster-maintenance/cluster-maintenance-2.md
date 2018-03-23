## Implement backup and restore methodologies
k8s的数据都存放在etcd中，因此只需要备份etcd的数据就能够完成k8s集群的备份与恢复。k8s官方提供了方便的工具去进行备份和恢复，[连接](https://kubernetes.io/docs/getting-started-guides/ubuntu/backups/)