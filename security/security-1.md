## Know how to configure authentication and authorization
K8s 集群主要有证书方式，静态Token方式等。对于外部请求进入API Server主要通过证书或者Token的方式，而内部Pod等与API通信都是通过ServiceAccount方式。

## 参考
- https://kubernetes.io/docs/admin/authentication/