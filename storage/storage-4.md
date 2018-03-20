## Understand Kubernetes Storage objects

在编写集群应用配置的时候：

	PVC对象和配置对象（Deployment、ConfigMap）写在一起；
	PV对象不和配置对象写在一起，使用配置对象的用户不一定有权限（admin）生成pv；
	

提供各种常用pv、pvc模板：

PV for NAS(Flexvolume):

```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-nas
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteMany
  flexVolume:
    driver: "alicloud/nas"
    options:
      server: "***-uih75.cn-hangzhou.nas.aliyuncs.com"
      path: "/"
      vers: "4.0"
      mode: "755"
```

PV for OSS(Flexvolume):

```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-oss
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteMany
  flexVolume:
    driver: "alicloud/oss"
    options:
      bucket: "***"
      url: "oss-cn-hangzhou.aliyuncs.com"
      otherOpts: "****"
```

PV for Disk(Flexvolume):

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
  flexVolume:
    driver: "alicloud/disk"
    fsType: "ext4"
    options:
      volumeId: "d-bp1j17ifxfasvts3tf40" 
```

PV for NFS:

```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: nfs
spec:
  capacity:
    storage: 1Mi
  accessModes:
    - ReadWriteMany
  nfs:
    server: 10.244.1.4
    path: "/exports"
```    
PV for Glusterfs:



PV for Ceph:

PV for HostPath:

PV for Iscsi:

PV for 