## <font color='red'> Helm </font>
Helm is a tool for managing Charts. Charts are packages of pre-configured Kubernetes resources. 

Helm 3.5 has already been installed and configured.

---

### <font color='red'> 1.1 Helm </font>
#### <font color='red'>IMPORTANT:</font> 
<strong>Please ensure you start with a clean environment. 
If you have previously run minikube, you will need to delete the existing instance.</strong>

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

--- 

additional minikube commands.  

upgrade cluster:
```
minikube start --kubernetes-version=latest
```
stop your local cluster:
```
minikube stop
```
> Minikube commands: https://minikube.sigs.k8s.io/docs/commands/

---

### <font color='red'> 1.2 Portainer </font>
Portainer is an open-source toolset that allows you to easily build and manage Containers in Docker, Swarm, Kubernetes and Azure ACI.

> To access Portainer: http://0.0.0.0:9000/#!/auth  

    * user: admin  
    * password: portainer

For further information:  

> Portainer docs: https://documentation.portainer.io/

---