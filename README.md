# k8s-cluster
Based on the idea from https://kubernetes.io/blog/2019/03/15/kubernetes-setup-using-ansible-and-vagrant/

Notes:
 1. kubectl label node node-1/node-2 node-role.kubernetes.io/node=  to label the nodes
 Check:
   kubectl get nodes

>>>>
vagrant@k8s-master:~$ kubectl get nodes
NAME         STATUS   ROLES    AGE     VERSION
k8s-master   Ready    master   9m46s   v1.16.1
node-1       Ready    node     6m23s   v1.16.1
node-2       Ready    node     3m10s   v1.16.1   
 

 
Check:
  kubectl get pods --all-namespaces 
>>>>
vagrant@k8s-master:~$ kubectl get pods --all-namespaces
NAMESPACE     NAME                                      READY   STATUS    RESTARTS   AGE
kube-system   calico-kube-controllers-b7fb7899c-dbkxn   1/1     Running   0          7m20s
kube-system   calico-node-79l2h                         1/1     Running   0          4m17s
kube-system   calico-node-bq66d                         1/1     Running   0          7m20s
kube-system   calico-node-j6lhk                         1/1     Running   0          64s
kube-system   coredns-5644d7b6d9-blkj4                  1/1     Running   0          7m20s
kube-system   coredns-5644d7b6d9-ctgwp                  1/1     Running   0          7m20s
kube-system   etcd-k8s-master                           1/1     Running   0          6m16s
kube-system   kube-apiserver-k8s-master                 1/1     Running   0          6m42s
kube-system   kube-controller-manager-k8s-master        1/1     Running   0          6m30s
kube-system   kube-proxy-8g8wn                          1/1     Running   0          4m17s
kube-system   kube-proxy-bh4wm                          1/1     Running   0          7m20s
kube-system   kube-proxy-kt6sf                          1/1     Running   0          64s
kube-system   kube-scheduler-k8s-master                 1/1     Running   0          6m34s
  

Port Forwarding:
===============
kubectl proxy --port=8080 --address=0.0.0.0

http://localhost:8080/api/  