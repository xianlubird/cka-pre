##Use label selectors to schedule Pods
- 获取当前集群所有节点，并找到某个节点的标签

```
[root@iZwz9jbz8fkh7uiqixg4ctZ cka]# kubectl get nodes
NAME                                 STATUS    ROLES     AGE       VERSION
cn-shenzhen.i-wz967jbqw9mmsaz6um14   Ready     <none>    15d       v1.9.3
cn-shenzhen.i-wz9dv05xs2393bg4gwhm   Ready     master    15d       v1.9.3
cn-shenzhen.i-wz9fzprkt1ul6m02dunl   Ready     master    15d       v1.9.3
cn-shenzhen.i-wz9jbz8fkh7uiqixg4ct   Ready     master    15d       v1.9.3
```
寻找node `cn-shenzhen.i-wz967jbqw9mmsaz6um14`所有标签

```
[root@iZwz9jbz8fkh7uiqixg4ctZ cka]# kubectl label node cn-shenzhen.i-wz967jbqw9mmsaz6um14 --list
failure-domain.beta.kubernetes.io/zone=cn-shenzhen-b
kubernetes.io/hostname=cn-shenzhen.i-wz967jbqw9mmsaz6um14
beta.kubernetes.io/arch=amd64
beta.kubernetes.io/instance-type=ecs.s2.large
beta.kubernetes.io/os=linux
failure-domain.beta.kubernetes.io/region=cn-shenzhen
```

在k8s官网找一个pods的模板，主要是寻找`nodeSelector `关键字，找到的模板如下:

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    env: test
spec:
  containers:
  - name: nginx
    image: nginx
    imagePullPolicy: IfNotPresent
  nodeSelector:
    disktype: ssd
```

修改该模板，让这个pod被指定调度到刚才的那个node 上

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx
    imagePullPolicy: IfNotPresent
  nodeSelector:
    kubernetes.io/hostname: cn-shenzhen.i-wz967jbqw9mmsaz6um14
```

查看部署结果

```
[root@iZwz9jbz8fkh7uiqixg4ctZ cka]# kubectl get pods -o wide
NAME      READY     STATUS    RESTARTS   AGE       IP             NODE
nginx     1/1       Running   0          1m        172.16.3.127   cn-shenzhen.i-wz967jbqw9mmsaz6um14
[root@iZwz9jbz8fkh7uiqixg4ctZ cka]# kubectl get nodes
NAME                                 STATUS    ROLES     AGE       VERSION
cn-shenzhen.i-wz967jbqw9mmsaz6um14   Ready     <none>    15d       v1.9.3
cn-shenzhen.i-wz9dv05xs2393bg4gwhm   Ready     master    15d       v1.9.3
cn-shenzhen.i-wz9fzprkt1ul6m02dunl   Ready     master    15d       v1.9.3
cn-shenzhen.i-wz9jbz8fkh7uiqixg4ct   Ready     master    15d       v1.9.3

```

可以看到，这个pod已经被调度到指定的那台节点上。
