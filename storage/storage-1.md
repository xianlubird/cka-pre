## Understand persistent volumes and know how to create them;

Kubernetes提供了2个资源类型来描述持久化卷；

	PersistentVolume (PV)：提供网络存储资源，是对硬件资源的直接描述；
	PersistentVolumeClaim (PVC)：PVC 请求存储资源，是对资源的抽象，供pod使用；

## 创建一个PV：

```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: d-bp1j17ifxfasvts3tf40
  labels:
    failure-domain.beta.kubernetes.io/zone: cn-hangzhou-c
spec:
  capacity:
    storage: 20Gi
  accessModes:
    - ReadWriteOnce
  storageClassName: alicloud-disk-common-zone
  persistentVolumeReclaimPolicy: Delete
  mountOptions:
    - hard
    - nfsvers=4.1
  nfs:
    path: /tmp
    server: 172.17.0.2
```

**storageClassName**: 创建pv时指定storageClassName，可以标识这个pv的类型，在与pvc进行匹配时会比较此字段；

**capacity**：标识创建PV的存储大小，以Mi、Gi为单位，不具备实际的约束意义。

**mountOptions**: 标识节点在执行mount的时候添加的mount options。


## 创建一个pvc：

```
kind: PersistentVolumeClaim
apiVersion: v1
metadata:
  name: pvc-disk
  labels:
    failure-domain.beta.kubernetes.io/zone: cn-hangzhou-c
spec:
  accessModes:
    - ReadWriteOnce
  volumeMode: Filesystem
  storageClassName: alicloud-disk-common-zone
  resources:
    requests:
      storage: 20Gi
  selector:
    matchLabels:
      release: "stable"
    matchExpressions:
      - {key: environment, operator: In, values: [dev]}
```

**volumeMode**：1.9新加feature，把一个volume挂载成文件系统（默认）或者块设备；

**selector**：定义filter，来筛选期望的pv；

**storageClassName**：定义Provisioner，通过Provisioner动态创建pv；如果系统中存储符合条件的pv，则不会再新创建pv；

## PV 与 PVC绑定：
动态存储：

	通过Provisioner生成的pv与源PVC会始终绑定；

静态存储，pvc会根据以下条件匹配：

	accessModes：完全匹配；
	storage：模糊匹配，但pv的storage 必须大于等于pvc的需求；
	label：完全匹配；
	storageClassName：完全匹配；