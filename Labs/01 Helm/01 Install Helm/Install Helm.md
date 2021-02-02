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

This section contains some tools you may find useful.

### <font color='red'> Portainer </font>
Portainer is an open-source toolset that allows you to easily build and manage Containers in Docker, Swarm, Kubernetes and Azure ACI.

to add Portainer to the Helm repo:
```
helm repo add portainer https://portainer.github.io/k8s/
```
update the repo:
```
helm repo update
```
create the namespace:
```
kubectl create namespace portainer
```
for load balancer:
```
helm install -n portainer portainer portainer/portainer --set service.type=LoadBalancer
```
for ingress:
```
helm install -n portainer portainer portainer/portainer --set service.type=ClusterIP
```
> To access Portainer: http://0.0.0.0:9000/#!/auth  

    * user: admin  
    * password: portainer

For further information:  

> Portainer docs: https://documentation.portainer.io/

---

### <font color='red'> Weave Scope </font>
Weave Scope automatically detects processes, containers, hosts.

download Weave Scope:
```
sudo curl -L git.io/scope -o /usr/local/bin/scope
```
change permissions:
```
sudo chmod a+x /usr/local/bin/scope
```
launch scope:
```
scope launch
```

> view in browser: http://<vm-IP address>:4040 

---