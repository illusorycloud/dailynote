# Kubernetes 网络隔离 NetworkPolicy

## 1. 概述

> [官方文档-network-policies](https://kubernetes.io/docs/concepts/services-networking/network-policies/)



在 Kubernetes 里，网络隔离能力的定义，是依靠一种专门的 API 对象来描述的，即：**NetworkPolicy**。

**Kubernetes 里的 Pod 默认都是“允许所有”（Accept All）的**。即：Pod 可以接收来自任何发送方的请求；或者，向任何接收方发送请求。

而如果你要对这个情况作出限制，就必须通过 NetworkPolicy 对象来指定。



## 2. 实例

一个完整的 NetworkPolicy 对象的示例，如下所示：

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: test-network-policy
  namespace: default
spec:
  podSelector:
    matchLabels:
      role: db
  policyTypes:
  - Ingress
  - Egress
  ingress:
  - from:
    - ipBlock:
        cidr: 172.17.0.0/16
        except:
        - 172.17.1.0/24
    - namespaceSelector:
        matchLabels:
          project: myproject
    - podSelector:
        matchLabels:
          role: frontend
    ports:
    - protocol: TCP
      port: 6379
  egress:
  - to:
    - ipBlock:
        cidr: 10.0.0.0/24
    ports:
    - protocol: TCP
      port: 5978
```

具体参数如下：

* **metadata.namespace** 指定要限制 default namespace 下的Pod
* **spec.podSelector.matchLabels** 用于匹配要限制的 Pod，这里就是限制有 role: db 这个标签的 Pod
* **policyTypes **指定要限制的类型，Ingress 为入流量，Egress为出流量。
* **ingress.from、egress.to** 则是白名单，匹配具体的IP或者Pod，有 ipBlock、namespaceSelector 和 podSelector 这三种指定方式。
  * ipBlock：特定的 IP CIDR 范围。
  * podSelector ：在 NetworkPolicy 指定的名字空间中选择特定的 Pod
  * namespaceSelector ：特定 namespace 里的所有 Pod
* **ports** 同样是白名单，用于匹配具体的端口号



**一旦 Pod 被 NetworkPolicy 匹配到了，那么这个 Pod 就会进入“拒绝所有”（Deny All）的状态，即：这个 Pod 既不允许被外界访问，也不允许对外界发起访问**。所以需要添加白名单。



### 注意事项

需要注意的是，定义一个 NetworkPolicy 对象的过程，容易犯错的是“白名单”部分（from 和 to 字段）。

下面这种写法是 OR 关系，满足任意一条即可 。

```yaml

  ...
  ingress:
  - from:
    - namespaceSelector:
        matchLabels:
          user: alice
    - podSelector:
        matchLabels:
          role: client
  ...
```

而这种写法则是 AND 关系，必须都满足才行。

```yaml
...
  ingress:
  - from:
    - namespaceSelector:
        matchLabels:
          user: alice
      podSelector:
        matchLabels:
          role: client
  ...
```





## 3. 小结

**如果要使上面定义的 NetworkPolicy 在 Kubernetes 集群里真正产生作用，你的 CNI 网络插件就必须是支持 Kubernetes 的 NetworkPolicy 的**。

在具体实现上，凡是支持 NetworkPolicy 的 CNI 网络插件，都维护着一个 NetworkPolicy Controller，通过控制循环的方式对 NetworkPolicy 对象的增删改查做出响应，然后在宿主机上完成 iptables 规则的配置工作。

**所以，Kubernetes 网络插件对 Pod 进行隔离，其实是靠在宿主机上生成 NetworkPolicy 对应的 iptable 规则来实现的**。

