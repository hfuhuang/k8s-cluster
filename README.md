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

Add Dashboard
===============
	kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0-beta4/aio/deploy/recommended.yaml

login:

	http://localhost:8080/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#/login

Dashboard Admin User
==
If you don't want to grant admin permission to dashboard service account, you can create cluster admin service account.

	$ kubectl create serviceaccount cluster-admin-dashboard-sa
	$ kubectl create clusterrolebinding cluster-admin-dashboard-sa --clusterrole=cluster-admin --serviceaccount=default:cluster-admin-dashboard-sa
  
And then, you can use the token of just created cluster admin service account.
	
	$ kubectl get secret | grep cluster-admin-dashboard-sa
	  cluster-admin-dashboard-sa-token-6xm8l   kubernetes.io/service-account-token   3         18m
	$ kubectl describe secret cluster-admin-dashboard-sa-token-6xm8l  
	
Helm
==

	vagrant@k8s-master:~$ helm init
	Creating /home/vagrant/.helm
	Creating /home/vagrant/.helm/repository
	Creating /home/vagrant/.helm/repository/cache
	Creating /home/vagrant/.helm/repository/local
	Creating /home/vagrant/.helm/plugins
	Creating /home/vagrant/.helm/starters
	Creating /home/vagrant/.helm/cache/archive
	Creating /home/vagrant/.helm/repository/repositories.yaml
	Adding stable repo with URL: https://kubernetes-charts.storage.googleapis.com
	Adding local repo with URL: http://127.0.0.1:8879/charts
	$HELM_HOME has been configured at /home/vagrant/.helm.
	Tiller (the Helm server-side component) has been installed into your Kubernetes Cluster.
	Please note: by default, Tiller is deployed with an insecure 'allow unauthenticated users' policy.
	To prevent this, run `helm init` with the --tiller-tls-verify flag.
	For more information on securing your installation see: https://docs.helm.sh/using_helm/#securing-your-helm-installation

In-Cluster Image Repository
==
* Ref: https://blog.container-solutions.com/installing-a-registry-on-kubernetes-quickstart (https://github.com/ContainerSolutions/trow.git)
* Or: https://github.com/helm/charts/tree/master/stable/docker-registry


In-Cluster Jenkins for CI/CD
==
t.b.d