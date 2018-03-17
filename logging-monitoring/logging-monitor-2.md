## Understand how to monitor applications
监控应用和监控系统组件没有什么区别，除了前面介绍的那些开源方案以外，还有一个基本的命令特别有用，那就是`kubectl top`。这个命令能够查看节点和pod的内存使用和cpu使用情况，对于应用的简单监控和信息获取特别有用。

- 获取当前各个节点的资源使用情况
```
[root@iZwz9jbz8fkh7uiqixg4ctZ ~]# kubectl top nodes
NAME                                 CPU(cores)   CPU%      MEMORY(bytes)   MEMORY%   
cn-shenzhen.i-wz967jbqw9mmsaz6um14   737m         36%       1537Mi          41%       
cn-shenzhen.i-wz9dv05xs2393bg4gwhm   239m         11%       2164Mi          58%       
cn-shenzhen.i-wz9fzprkt1ul6m02dunl   542m         27%       2234Mi          60%       
cn-shenzhen.i-wz9jbz8fkh7uiqixg4ct   476m         23%       2140Mi          57%    
```

- 获取指定pod的资源使用情况
```
[root@iZwz9jbz8fkh7uiqixg4ctZ ~]# kubectl top pods
NAME       CPU(cores)   MEMORY(bytes)   
cpu-demo   499m         0Mi             
```

同时对于监控节点的状态，k8s官方提供了几个镜像，专门用来诊断节点异常的原因和Kernel异常的原因，比较建议默认启动。