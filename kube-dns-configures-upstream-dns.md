## kube-dns configures upstream DNS

So that pods in kubernetes can visit Internet without host network.  
This feature comes with Kubernetes v1.6

![](/assets/import.png)

kubectl create -f kube-dns-configmap.yaml

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: kube-dns
  namespace: kube-system
data:
  upstreamNameservers: |
    ["xx.xx.xx.xx"]
```

kubectl edit deployment kube-dns -n kube-system

```
      ……
      dnsPolicy: ClusterFirst
      ……
      volumes:
      - configMap:
          defaultMode: 420
          name: kube-dns
          optional: true
        name: kube-dns-config
```

**Reference:**  
[http://blog.kubernetes.io/2017/04/configuring-private-dns-zones-upstream-nameservers-kubernetes.html](http://blog.kubernetes.io/2017/04/configuring-private-dns-zones-upstream-nameservers-kubernetes.html)

