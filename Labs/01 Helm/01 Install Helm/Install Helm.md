## <font color='red'> Helm </font>
Helm is a tool for managing Charts. Charts are packages of pre-configured Kubernetes resources. 

Helm 3.5 has already been installed and configured.

---

### <font color='red'> 1.1 Minikube </font>
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

### <font color='red'> 1.2 Helm </font>
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