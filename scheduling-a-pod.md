## Scheduling a pod

Pods can be placed onto a particular node in a number of ways. This case study demonstrates how the above design can be applied to satisfy the objectives.

### A pod scheduled by a user through the apiserver

1. A user submits a pod with Namespace="" and Name="guestbook" to the apiserver.
2. The apiserver validates the input.
   1. A default Namespace is assigned.
   2. The pod name must be space-unique within the Namespace.
   3. Each container within the pod has a name which must be space-unique within the pod.
3. The pod is accepted.
   1. A new UID is assigned.
4. The pod is bound to a node.
   1. The kubelet on the node is passed the pod's UID, Namespace, and Name.
5. Kubelet validates the input.
6. Kubelet runs the pod.
   1. Each container is started up with enough metadata to distinguish the pod from whence it came.
   2. Each attempt to run a container is assigned a UID \(a string\) that is unique across time. \* This may correspond to Docker's container ID.

### A pod placed by a config file on the node \(under /etc/kubernetes/manifests/\)

1. A config file is stored on the node, containing a pod with UID="", Namespace="", and Name="cadvisor".
2. Kubelet validates the input.
   1. Since UID is not provided, kubelet generates one.
   2. Since Namespace is not provided, kubelet generates one.
      1. The generated namespace should be deterministic and cluster-unique for the source, such as a hash of the hostname and file path.
         * E.g. Namespace="file-f4231812554558a718a01ca942782d81"
3. Kubelet runs the pod.
   1. Each container is started up with enough metadata to distinguish the pod from whence it came.
   2. Each attempt to run a container is assigned a UID \(a string\) that is unique across time.
      1. This may correspond to Docker's container ID.

### Reference

https://github.com/kubernetes/community/blob/master/contributors/design-proposals/architecture/identifiers.md



