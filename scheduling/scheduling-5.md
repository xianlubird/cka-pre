## Manually schedule a pod without a scheduler
 目前不通过scheduler创建pods的方式就是静态Pods。这种方式可以在集群的scheduler进程还未启动的时候，就在对应的节点上启动pods。

其实这种方式是通过节点上运行的`kubelet`来完成的。`kubelet`进程在启动的时候，会有一个参数`--pod-manifest-path`，这个参数就是用来指定`kubelet`进程监听的文件路径。当在这个路径下放置pods的yaml文件时，`kubelet`就会自动的创建对应的pods，但是`kubelet`进程不提供健康检查的功能，只能在容器crash的时候重启容器，这个功能一般用于在初始化集群的时候操作。

首先登录到k8s集群的某一个节点上，找到`kubelet`进程监听的文件路径，将如下模板放置到目录下：
```
cat <<EOF >/etc/kubelet.d/static-web.yaml
apiVersion: v1
kind: Pod
metadata:
  name: static-web
  labels:
    role: myrole
spec:
  containers:
    - name: web
      image: nginx
      ports:
        - name: web
          containerPort: 80
          protocol: TCP
EOF
```
稍等一会就能看到pods被创建
```
# kubectl get pods
NAME                                            READY     STATUS    RESTARTS   AGE
static-web-cn-shenzhen.i-wz9jbz8fkh7uiqixg4ct   1/1       Running   0          14m

```
静态pods是不能够被api server管理的，因此，无法通过`kubectl delete pods`来完成pods的删除。想要删除pods可以将对应的yaml文件移走，一会`kubelet`就会删除对应的pods。

```
[root@iZwz9jb]# mv static-web.yaml /tmp/
[root@iZwz9jb]# kubectl get pods
No resources found.

```

## 参考
- https://kubernetes.io/docs/tasks/administer-cluster/static-pod/