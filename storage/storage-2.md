## Understand Access Modes for volumes


Access Mode：访问模式

```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: d-bp1j17ifxfasvts3tf40
spec:
  capacity:
    storage: 20Gi
  accessModes:
    - ReadWriteOnce
  nfs:
    path: /tmp
    server: 172.17.0.2
```

	ReadWriteOnce：该卷能够以读写模式被加载到一个节点上。
	ReadOnlyMany：该卷能够以只读模式加载到多个节点上。
	ReadWriteMany：该卷能够以读写模式被多个节点同时加载。

一种卷类型可以支持多种访问模式，但是对于一个实例化的pv，只能支持其定义的那一种访问模式；


**测试发现：**
ReadWriteOnce模式只有部分pv类型（gce、aws、rbd）支持，其他类型即使定义了ReadWriteOnce模式，依然可以实现多个pod同时挂载；