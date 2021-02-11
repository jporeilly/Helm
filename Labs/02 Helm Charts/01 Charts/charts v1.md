## <font color='red'> 2.1 Helm Charts </font>
In these you're going to cover:
* Build a Helm Chart
* Release Guestbook v1
* Upgrade to Guestbook v1.1
* Rollback
* Uninstall

### <font color='red'> 2.1.1 Helm Charts </font>
in this Lab you will build a Guestbook Helm Chart.  
Ensure you're in the correct directory.

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
appVersion: "1.0"
description: A Helm chart for Guestbook 1.0 
name: guestbook
version: 0.1.0
type: application
```
create a templates directory:
```
sudo mkdir templates
```
copy over the yaml files:
```
sudo cp ../yaml/*.yaml templates
```
so you should have the following structure:  

> guestbook  
>>   Chart.yaml  
>>   templates
>>>    frontend-service.yaml  
>>>    frontend.yaml  
>>>    ingress.yaml  


---

### <font color='red'> 2.1.2 Deploy with Helm Chart </font>
in this lab you will deploy the Guestbook v1 app.

ensure you have a clean install:
```
minikube stop
```
delete the minikube cluster:
```
minikube delete
```  
start minikube:
```
minikube start
```  

create a namespace:
```
kubectl create namespace helm-demo
```   
  
to deploy Guestbook v1 app:
```
helm install guestbook-demo ./guestbook/ --namespace helm-demo
```
check deployment:
```
kubectl get all
```
check frontend POD:
```
kubectl get pod -l app=frontend
```
to get the names of installed releases:
```
helm list --short  -n helm-demo
```
look at the manifest:
```
helm get manifest guestbook-demo -n helm-demo | less
```

> check in browser: http://frontend.minikube.local/guestbook

if you problems resolving the URL you may need to update the /etc/hosts with frontend POD IP

---

### <font color='red'> 2.1.3 Upgrade with Helm Chart </font>
new release of the guestbook v1.1.
ensure you're in the correct directory.

edit Chart.yaml file:
```
sudo nano Chart.yaml
```
change the following the following:
```
appVersion: "1.1"
description: A Helm chart for Guestbook 1.1 
name: guestbook
version: 0.1.0
```
edit frontend.yaml to update release:
```
- image: jporeilly/frontend:1.1
```
to upgrade to Guestbook v1.1:
```
helm upgrade guestbook-demo ./guestbook/ --namespace helm-demo
```
check that the new image is used:
```
kubectl describe pod -l app=frontend -n helm-demo
```
check the revision:
```
helm status guestbook-demo -n helm-demo
```

> check in browser: http://frontend.minikube.local/guestbook

---

### <font color='red'> 2.1.4 Rollback with Helm Chart </font>
rollback to Guestbook v1.

to rollback:
```
helm rollback guestbook-demo 1 -n helm-demo
```
to view the history:
```
helm history guestbook-demo -n helm-demo
```

> check in browser: http://frontend.minikube.local/guestbook

to delete all revisions:
```
helm uninstall guestbook-demo -n helm-demo
```

---