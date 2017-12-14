## leader election

#### 1. 什么是 leader election

在分布式计算中，从多个节点中选举一个leader

\[[https://en.wikipedia.org/wiki/Leader\_election](https://en.wikipedia.org/wiki/Leader_election)\]

#### 2. leader election example

启动 leader elector 容器：

```bash
$ kubectl run leader-elector --image=gcr.io/google_containers/leader-elector:0.5 --replicas=3 -- --election=example​
```

k8s 启动了3个容器：

```
$ kubectl get pods
NAME                   READY     STATUS    RESTARTS   AGE
leader-elector-inmr1   1/1       Running   0          13s
leader-elector-qkq00   1/1       Running   0          13s
leader-elector-sgwcq   1/1       Running   0          13s
```

可以通过 kubectl logs 查看容器日志：

```
$ kubectl logs -f ${pod_name}
```

现在 k8s 集群中有一个叫 example 的 endpoints：

```
# ‘example’ is the name of the candidate set from the above kubectl run … command
$ kubectl get endpoints example -o yaml
apiVersion: v1
kind: Endpoints
metadata:
  annotations:
    control-plane.alpha.kubernetes.io/leader: '{"holderIdentity":"leader-elector-1591020587-inmr1","leaseDurationSeconds":10,"acquireTime":"2017-11-07T07:17:13Z","renewTime":"2017-11-16T06:36:32Z","leaderTransitions":0}'
  creationTimestamp: 2017-11-07T06:59:10Z
  name: example
  namespace: default
  resourceVersion: "37362459"
  selfLink: /api/v1/namespaces/default/endpoints/example
  uid: 280b4d20-c389-11e7-95b9-9418820918cc
subsets: []
​
```

可以看到 "leader-elector-1591020587-inmr1" 就是当前的 leader

删除这个容器之后，k8s会重启一个新的容器并且重新选举 leader

通过查看代码 \[[https://github.com/kubernetes/contrib/tree/master/election](https://github.com/kubernetes/contrib/tree/master/election)\] 可以看到，每次选举，选举的内容就放在这个叫 example 的 endpoints 里 （选举哪个 leader 的算法这里就先忽略不谈），有过期时间，每个节点从 example 中得知 leader 信息，过期重选。保证这几个容器中只有一个 leader。

#### 3. leader election 在 ingress-nginx-controller 中的应用

ingress-nginx-controller 在 [status.go](https://github.com/kubernetes/ingress-nginx/blob/nginx-0.9.0-beta.17/pkg/ingress/status/status.go) 中用到了 leader election。ingress-nginx-controller 会 watch k8s 集群中 ingress 的状态，并且生成相应的配置文件。一般 k8s 中会有多个 ingress-nginx-controller，为了保证数据的统一，需要选举出一个 leader 进行操作。

#### 4. leader election 在 kube-controller, kube-scheduler 中的应用

[https://kubernetes.io/docs/admin/high-availability/\#master-elected-components](https://kubernetes.io/docs/admin/high-availability/#master-elected-components)



