## Know how to scale applications
Scale 应用最简单的方式就是使用`kubectl scale`命令，此命令可以scale Deployment StatefulSet等控制器，是最方便的扩容应用的方式。


另外一种是HPA方式，就是自动根据应用资源占用情况，来动态的扩容或者缩容。目前可以通过API-Server的方式获取对应的资源，然后进行动态的调整。基本使用可以类似下面这种方式`kubectl autoscale rc foo --min=2 --max=5 --cpu-percent=80`,这样就可以在这个应用CPU资源占用率在百分之80的时候扩容这个应用，最大到5个实例，最小到2个。

## 参考
- https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/