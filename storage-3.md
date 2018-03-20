Understand persistent volume claims primitive

## PVC介绍：

PVC 是对一个存储卷的声明，会匹配符合条件的PV；

```
kind: PersistentVolumeClaim
apiVersion: v1
metadata:
  name: myclaim
spec:
  accessModes:
    - ReadWriteOnce
  volumeMode: Filesystem
  persistentVolumeReclaimPolicy: Retain
  resources:
    requests:
      storage: 8Gi
  storageClassName: slow
  selector:
    matchLabels:
      release: "stable"
    matchExpressions:
      - {key: environment, operator: In, values: [dev]}
```

**persistentVolumeReclaimPolicy:** 回收策略，支持Retain、Recycle、Delete；

	Retain：PV不作处理，等待操作人员手工处理；
	Recycle：PV复用，会执行删除命令(rm -rf /thevolume/*)，新版已经建议删除；
	Delete：删除操作，动态存储的默认模式，只有特定的存储类型支持；

**AccessMode**: 访问模式，同PV；

**volumeMode**：挂载文件系统，同pv；目前只有fibre channel一种类型支持Block；

**resources**：资源定义，目前只支持storage，以后会支持iops等；

**selector**：设置过滤条件，符合条件的pv会被绑定；

**storageClassName**：指定生成存储卷的Provisioner；只有同pvc具有相同storageClassName的pv才能绑定；

## 在Pod中使用pvc：

pod和pvc必须在相同的namespaces中；以many的方式mount的时候也只能发生在同一个namespaces；

```
kind: Pod
apiVersion: v1
metadata:
  name: mypod
spec:
  containers:
    - name: myfrontend
      image: dockerfile/nginx
      volumeMounts:
      - mountPath: "/var/www/html"
        name: mypd
  volumes:
    - name: mypd
      persistentVolumeClaim:
        claimName: myclaim
```


