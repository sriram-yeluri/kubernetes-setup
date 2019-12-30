# kubernetes-setup
kubernetes-setup with vagrant and ansible

1 master
2 workers

```sh
# to create k8s cluster
vagrant up

# to halt and resume the cluster.
vagrant suspend
vagrant resume

# to destroy all vms
vagrant destroy -f
```


```sh
## Accessing master
vagrant ssh k8s-master
kubectl get nodes

vagrant@k8s-master:~$ kubectl get nodes
NAME         STATUS   ROLES    AGE     VERSION
k8s-master   Ready    master   6m56s   v1.17.0
node-1       Ready    <none>   4m11s   v1.17.0
node-2       Ready    <none>   102s    v1.17.0

vagrant@k8s-master:~$ kubectl get pods -n kube-system
NAME                                       READY   STATUS    RESTARTS   AGE
calico-kube-controllers-778676476b-bl4h4   1/1     Running   0          7m2s
calico-node-5dj2k                          1/1     Running   0          4m36s
calico-node-6zdl2                          1/1     Running   0          2m6s
calico-node-rgwkz                          1/1     Running   1          7m2s
coredns-6955765f44-f76pf                   1/1     Running   0          7m2s
coredns-6955765f44-p547s                   1/1     Running   0          7m2s
etcd-k8s-master                            1/1     Running   0          7m18s
kube-apiserver-k8s-master                  1/1     Running   0          7m18s
kube-controller-manager-k8s-master         1/1     Running   0          7m18s
kube-proxy-7tkqf                           1/1     Running   0          7m2s
kube-proxy-jxmfs                           1/1     Running   0          2m6s
kube-proxy-lgfjs                           1/1     Running   0          4m36s
kube-scheduler-k8s-master                  1/1     Running   0          7m18s
```

```sh
## # Accessing nodes
vagrant ssh node-1
vagrant ssh node-2
```

```sh
# running a cluster with kubeadm on virtual machine, below steps are needed.
vagrant ssh node-1
ip a # get the ip-address of node
sudo vi /etc/systemd/system/kubelet.service.d/10-kubeadm.conf
Environment="KUBELET_EXTRA_ARGS=--node-ip=<worker IP address>"
sudo systemctl daemon-reload
sudo systemctl restart kubelet

kubectl get nodes -o wide
# ip of node will change, now we can access logs and connect to pods with exec.
```
