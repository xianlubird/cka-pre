## Display scheduler events
当创建Deployment或者其他情况的时候，我们希望能够获得对应的事件，看看到底发生了什么，有两种获得事件的方式。
- 直接使用`kubectl describe <resource>`方式

```
[root@iZwz9jbz8fkh7uiqixg4ctZ cka]# kubectl describe pods
Name:         cpu-demo
Namespace:    default
Node:         <none>
Labels:       <none>
Annotations:  <none>
Status:       Pending
IP:           
Containers:
  cpu-demo-ctr:
    Image:  vish/stress
    Port:   <none>
    Args:
      -cpus
      2
    Limits:
      cpu:  100
    Requests:
      cpu:        100
    Environment:  <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-lvprh (ro)
Conditions:
  Type           Status
  PodScheduled   False 
Volumes:
  default-token-lvprh:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-lvprh
    Optional:    false
QoS Class:       Burstable
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
Events:
  Type     Reason            Age               From               Message
  ----     ------            ----              ----               -------
  Warning  FailedScheduling  4s (x6 over 19s)  default-scheduler  0/4 nodes are available: 3 PodToleratesNodeTaints, 4 Insufficient cpu.

```
可以看到，最下方就是这个deployment的事件，这里就能看到，因为没有足够的资源导致调度失败。

- 使用`kubectl get events`方式

```
[root@iZwz9jbz8fkh7uiqixg4ctZ cka]# kubectl get events
LAST SEEN   FIRST SEEN   COUNT     NAME                                                             KIND      SUBOBJECT              TYPE      REASON             SOURCE                                        MESSAGE
31s         3m           15        cpu-demo.151bcf5ec4d514e5                                        Pod                              Warning   FailedScheduling   default-scheduler                             0/4 nodes are available: 3 PodToleratesNodeTaints, 4 Insufficient cpu.
30m         30m          1         static-web-cn-shenzhen.i-wz9jbz8fkh7uiqixg4ct.151bcde3537f5a36   Pod       spec.containers{web}   Normal    Pulling            kubelet, cn-shenzhen.i-wz9jbz8fkh7uiqixg4ct   pulling image "nginx"
30m         30m          1         static-web-cn-shenzhen.i-wz9jbz8fkh7uiqixg4ct.151bcde54047ca47   Pod       spec.containers{web}   Normal    Pulled             kubelet, cn-shenzhen.i-wz9jbz8fkh7uiqixg4ct   Successfully pulled image "nginx"

```
可以看到，最上方就是调度器的事件，这种方式能够看到一些集群级别的事件，更广泛一些。