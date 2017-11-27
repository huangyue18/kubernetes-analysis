## Garbage collection

#### Owner References

1. create a busybox deployment

   ```bash
   $ kubectl run busybox --image=busybox --command -- sleep 3600
   ```

2. check busybox pod

   ```bash
   $ kubectl get pod busybox-2474944738-prm8x -o yaml
   apiVersion: v1
   kind: Pod
   metadata:
     ...
     name: busybox-2474944738-prm8x
     namespace: default
     ownerReferences:
     - apiVersion: extensions/v1beta1
       blockOwnerDeletion: true
       controller: true
       kind: ReplicaSet
       name: busybox-2474944738
       uid: 2d0d9564-d355-11e7-95b9-9418820918cc
     resourceVersion: "40324552"
     selfLink: /api/v1/namespaces/default/pods/busybox-2474944738-prm8x
     uid: 2d0fb405-d355-11e7-95b9-9418820918cc
   spec:
     ...
   ```

   The pod's owner reference points to a ReplicaSet

3. check busybox ReplicaSet

   ```bash
   $ kubectl get replicaset busybox-2474944738 -o yaml
   apiVersion: extensions/v1beta1
   kind: ReplicaSet
   metadata:
     ...
     name: busybox-2474944738
     namespace: default
     ownerReferences:
     - apiVersion: extensions/v1beta1
       blockOwnerDeletion: true
       controller: true
       kind: Deployment
       name: busybox
       uid: 2d0c4224-d355-11e7-95b9-9418820918cc
     resourceVersion: "40324553"
     selfLink: /apis/extensions/v1beta1/namespaces/default/replicasets/busybox-2474944738
     uid: 2d0d9564-d355-11e7-95b9-9418820918cc
   spec:
     ...
   ```

   The ReplicaSet's owner reference points to a Deployment

4. check busybox Deployment

5. delete Deployment with option "cascading==false"

6. delete ReplicaSet without option \(by default "cascading==true"\)

#### Codes \(kubelet\)

In k8s v1.8.x version, cascading delete logics are implemented on client side:  
kubernetes/pkg/kubectl/cmd/delete.go

#### References

[https://kubernetes.io/docs/concepts/workloads/controllers/garbage-collection/](https://kubernetes.io/docs/concepts/workloads/controllers/garbage-collection/)  
[https://git.k8s.io/community/contributors/design-proposals/api-machinery/garbage-collection.md](https://git.k8s.io/community/contributors/design-proposals/api-machinery/garbage-collection.md)  
[https://git.k8s.io/community/contributors/design-proposals/api-machinery/synchronous-garbage-collection.md](https://git.k8s.io/community/contributors/design-proposals/api-machinery/synchronous-garbage-collection.md)

