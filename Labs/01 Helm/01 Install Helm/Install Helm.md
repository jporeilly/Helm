## <font color='red'> Helm 3</font>
Helm is a tool for managing Charts. Charts are packages of pre-configured Kubernetes resources. 

Helm 3.5 has already been installed and configured.

---

### <font color='red'> 1.1.1 Minikube </font>
#### <font color='red'>IMPORTANT:</font> 
<strong>Please ensure you start with a clean environment. 
If you have previously run minikube, in another course, you will need to delete the existing instance.</strong>

to delete  minikube:
```
minikube delete
```

to start minikube:
```
minikube start
```

check minikube status:
```
minikube status
```

view addons:
```
minikube addons list
```

For additional insight into your cluster state, minikube bundles the Kubernetes Dashboard:
in a new terminal access dashboard:
```
minikube dashboard
```
> For further information: https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/

> Blog: https://www.replex.io/blog/how-to-install-access-and-add-heapster-metrics-to-the-kubernetes-dashboard

> Minikube commands: https://minikube.sigs.k8s.io/docs/commands/

---

### <font color='red'> 1.1.2 Helm </font>
Helm server has been installed to /usr/local/bin/helm

check helm:
```
helm version --short
```
to add the helm repositories:
```
helm repo add stable https://charts.helm.sh/stable
```

--- 

### <font color='red'> 1.1.3 Quickstart </font>
To show how easy it is to install an application - MySQL

update Helm repo:
```
helm repo update              # Make sure we get the latest list of charts
```
to install MySQL:
```
helm install stable/mysql --generate-name
```
check MySQL:
```
kubectl get all | grep mysql
```
also can use:
```
helm ls
```

---

### <font color='red'> 1.1.4 Cleanup </font>

can also check the secrets:
```
kubectl get secret | grep mysql
```
helm uninstall:
```
helm uninstall mysql-xxxxxxxxxx
```
to see the POD terminating:
```
kubectl get all | grep mysql
```
can also check the secrets:
```
kubectl get secret | grep mysql
```
nothing...  
  
also caches some configuraion variables:
```
helm env
```  

---