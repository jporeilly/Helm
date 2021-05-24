## <font color='red'> 2.2 Umbrella Helm Charts </font>
In these Labs you're going to cover:
* Building an Umbrella Helm Chart
* Deploy Guestbook v2 with a Helm Chart

### <font color='red'> Helm Umbrella Charts </font>
In this Lab you will build an umbrella Guestbook v2 Helm Chart.  
Ensure you're in the correct directory if you're using the integrated terminal.

create a guestbook directory:
```
sudo mkdir guestbook
```
change to guestbook directory:
```
cd guestbook
```
add a Chart.yaml file:
```
sudo nano Chart.yaml
```
add the following:
```
apiVersion: v2
appVersion: "2.0"
description: A Helm chart for Guestbook 2.0 
name: guestbook
version: 1.1.0
type: application
```

create a charts directory:
```
sudo mkdir charts
```
change to charts:
```
cd charts
```
you will now need to create the 3 app directories:
```
sudo mkdir frontend && sudo mkdir backend && sudo mkdir database
```
change to frontend:
```
cd frontend
```
add a Chart.yaml file:
```
sudo nano Chart.yaml
```
add the following:
```
apiVersion: v2
appVersion: "2.0"
description: A Helm chart for Guestbook Frontend 2.0 
name: frontend
version: 1.1.0
type: application
```
create a templates directory:
```
sudo mkdir templates
```
copy over the yaml files:
```
sudo cp ../../../yaml/frontend*.yaml templates 
sudo cp ../../../yaml/ingress.yaml templates
```
check the directory structure:
```
tree guestbook
```
<details>
  <summary>Click to expand Guestbook v2 tree!</summary>
 
> guestbook   
> Chart.yaml 

<details>
  <summary>charts</summary>

>>  frontend  
   </details>

<details>
  <summary>templates </summary>

>>>  Chart.yaml  
>>>>    frontend-configMap.yaml  
>>>>    frontend-service.yaml  
>>>>    frontend.yaml  
>>>>    ingress.yaml  
   </details>
</details>  
<br/>
repeat the workflow with the following Chart.yaml:  

Chart.yaml for database:  
```
apiVersion: v2
appVersion: "3.6"
description: A Helm chart for Guestbook Database Mongodb 3.6 
name: database
version: 1.1.0
type: application
```
Chart.yaml for backend:  
```
apiVersion: v2
appVersion: "2.0"
description: A Helm chart for Guestbook Backend 2.0 
name: backend
version: 1.1.0
type: application
```

keep checking the structure:
```
tree guestbook
```     

---

### <font color='red'> Deploy with Helm Chart </font>
in this lab you will deploy the Guestbook v2 app.

create a namespace:
```
kubectl create namespace helm-demo
```   
  
to deploy Guestbook v2 app:
```
helm install guestbook-demo ./guestbook/ --namespace helm-demo
```
check deployment:
```
kubectl get all -n helm-demo
```
to get the names of installed releases:
```
helm list -n helm-demo
```

> check in browser: http://frontend.minikube.local/guestbook

if you problems resolving the URL you may need to update the /etc/hosts with:
[frontend-service IP] frontend.minikube.local
[database-service IP] database.minikube.local
[backend.minikube.local IP] backend.minikube.local

to delete all revisions:
```
helm uninstall guestbook-demo -n helm-demo
```

---