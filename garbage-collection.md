## Garbage collection

#### Owner Reference

1. create a busybox deployment

   ```bash
   $ kubectl run busybox --image=busybox --command -- sleep 3600
   ```

2. check busybox pod

#### Codes \(kubelet\)

#### References

[https://kubernetes.io/docs/concepts/workloads/controllers/garbage-collection/](https://kubernetes.io/docs/concepts/workloads/controllers/garbage-collection/)  
[https://git.k8s.io/community/contributors/design-proposals/api-machinery/garbage-collection.md](https://git.k8s.io/community/contributors/design-proposals/api-machinery/garbage-collection.md)  
[https://git.k8s.io/community/contributors/design-proposals/api-machinery/synchronous-garbage-collection.md](https://git.k8s.io/community/contributors/design-proposals/api-machinery/synchronous-garbage-collection.md)

