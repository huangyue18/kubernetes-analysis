## Upgrade Kubernetes from v1.7.x to v1.8.5

1. prepare rpm packages & images  
   rpm packages:  
       kubeadm-1.8.5-0.x86\_64.rpm  
       kubelet-1.8.5-0.x86\_64.rpm  
       kubectl-1.8.5-0.x86\_64.rpm

   images:  
       gcr.io/google\_containers/kube-apiserver-amd64:v1.8.5  
       gcr.io/google\_containers/kube-apiserver-amd64:v1.8.5  
       gcr.io/google\_containers/kube-apiserver-amd64:v1.8.5  
       gcr.io/google\_containers/kube-apiserver-amd64:v1.8.5  
       gcr.io/google\_containers/k8s-dns-kube-dns-amd64:1.14.5  
       gcr.io/google\_containers/k8s-dns-dnsmasq-nanny-amd64:1.14.5  
       gcr.io/google\_containers/k8s-dns-sidecar-amd64:1.14.5

2. upgrade master kubeadm  
   `yum install kubeadm`

3. upgrade master k8s core images  
   `kubeadm config upload from-file --config /etc/kubernetes/kubeadm.conf  
    kubeadm upgrade plan  
    kubeadm upgrade apply v1.8.5`

4. upgrade kubelet & kubectl on master  
   `yum install kubelet kubectl`

   edit /etc/systemd/system/kubelet.service.d/10-kubeadm.conf:  
   `Environment="KUBELET_CGROUP_ARGS=--cgroup-driver=cgroupfs"  
    Environment="KUBELET_EXTRA_ARGS=--fail-swap-on=false”`  
  
   `systemctl daemon-reload  
    systemctl restart kubelet`

5. upgrade kubelet on slaves  
   `swapoff -a  
    yum install kubelet  
    systemctl daemon-reload  
    systemctl restart kubelet`



**Known issues:**  
    upgrading kubelet will force all pods on that node restarting

**References:**  
    [https://kubernetes.io/docs/tasks/administer-cluster/kubeadm-upgrade-1-8/](https://kubernetes.io/docs/tasks/administer-cluster/kubeadm-upgrade-1-8/)  
    [https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG-1.8.md\#before-upgrading](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG-1.8.md#before-upgrading)





