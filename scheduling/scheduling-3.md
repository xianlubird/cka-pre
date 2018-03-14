## Understand how resource limits can affect Pod scheduling
`Pod`可以通过`resources `下的`limits `标签，限制容器可以使用的内存或者CPU，通过`requests `表明容器需要多少内存或者 CPU。调度系统会根据系统节点的资源量，保证节点有足够的资源留给对应的pod。
> CPU requests and limits are associated with Containers, but it is useful to think of a Pod as having a CPU request and limit. The CPU request for a Pod is the sum of the CPU requests for all the Containers in the Pod. Likewise, the CPU limit for a Pod is the sum of the CPU limits for all the Containers in the Pod.

> Pod scheduling is based on requests. A Pod is scheduled to run on a Node only if the Node has enough CPU resources available to satisfy the Pod’s CPU request.

一个简单的针对pod限制内存和cpu的例子模板如下:

```
apiVersion: v1
kind: Pod
metadata:
  name: cpu-demo-2
  namespace: cpu-example
spec:
  containers:
  - name: cpu-demo-ctr-2
    image: vish/stress
    resources:
      limits:
        cpu: "1"
      requests:
        cpu: "0.5"
    args:
    - -cpus
    - "2"
```

## reference

- [https://kubernetes.io/docs/tasks/configure-pod-container/assign-cpu-resource/](https://kubernetes.io/docs/tasks/administer-cluster/memory-default-namespace/)
- [https://kubernetes.io/docs/tasks/administer-cluster/memory-default-namespace/](https://kubernetes.io/docs/tasks/administer-cluster/memory-default-namespace/)