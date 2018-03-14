## Understand the role of DaemonSets
参考文档了解详情
[DaemonSets](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/)

一个简单的DaemonSets

```
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: mynginx
  labels:
    k8s-app: nginx
spec:
  selector:
    matchLabels:
      name: nginx
  template:
    metadata:
      labels:
        name: nginx
    spec:
      tolerations:
      - key: node-role.kubernetes.io/master
        effect: NoSchedule
      containers:
      - name: mynginx
        image: nginx
        resources:
          limits:
            memory: 200Mi
          requests:
            cpu: 100m
            memory: 200Mi
      terminationGracePeriodSeconds: 30
```

关于`Tolerations `参考 [Tolerations](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/)