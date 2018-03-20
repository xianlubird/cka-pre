## Understand the primitivies necessary to create a self-healing application
自动恢复的pod，目前Deployment 或者ReplicaSet等，你只要指定了具体的`replica`数目，控制器就会默认帮你维持这个pod的数量，如果有一个pod 因为某种原因挂掉，控制器就会在其他地方帮你自动的在创建出来一个新的pod，一直保持pod数量的稳定性。这就是控制器的目的所在。

另外对于有顺序的创建和删除，需要参考[StatefulSets](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/)