## Understand how to run multiple schedulers and how to configure Pods to use them
主要是通过`spec`下的`schedulerName`来指定到底使用哪个scheduler去调度容器，参考yaml:

```
apiVersion: v1
kind: Pod
metadata:
  name: annotation-second-scheduler
  labels:
    name: multischeduler-example
spec:
  schedulerName: my-scheduler
  containers:
  - name: pod-with-second-annotation-container
    image: k8s.gcr.io/pause:2.0
```

## 参考
- https://kubernetes.io/docs/tasks/administer-cluster/configure-multiple-schedulers/