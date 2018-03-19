## Understand Deployment and how to perform rolling updates and rollbacks
一个最简单的Deplyment大概长得样子如下：
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.7.9
        ports:
        - containerPort: 80
```
Deployment 会被创建出子ReplicaSets去创建对应的Pod，因此每一个Deployment都会有一个或者多个对应的ReplicaSet。

当创建一个Deployment失败或者需要回滚的时候，可以通过`kubectl rollout`命令来获取当前一共有多少个历史版本，然后指定对应的版本来回滚。

```
$ kubectl rollout history deployment/nginx-deployment
deployments "nginx-deployment"
REVISION    CHANGE-CAUSE
1           kubectl create -f docs/user-guide/nginx-deployment.yaml --record
2           kubectl set image deployment/nginx-deployment nginx=nginx:1.9.1
3           kubectl set image deployment/nginx-deployment nginx=nginx:1.91
```

回滚一个Deployment
```
$ kubectl rollout undo deployment/nginx-deployment
deployment "nginx-deployment" rolled back
```
或者指定一个版本去回滚
```
$ kubectl rollout undo deployment/nginx-deployment --to-revision=2
deployment "nginx-deployment" rolled back
```

当需要对一个Deployment进行对此变更的时候，可以先冻结Deployment，然后全部改变之后，在解冻。

```
$ kubectl rollout pause deployment/nginx-deployment
deployment "nginx-deployment" paused
```

恢复Deployment
```
$ kubectl rollout resume deploy/nginx-deployment
deployment "nginx" resumed
```

## 参考
- https://kubernetes.io/docs/concepts/workloads/controllers/deployment/