## <font color='red'> 2.2 Umbrella Helm Charts </font>
In these Labs you're going to cover:
* Building an Umbrella Helm Chart
* Deploy Guestbook v2 with a Helm Chart

### <font color='red'> 2.1.1 Helm Charts </font>
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
> guestbook
>>   Chart.yaml
>>   templates
>>>      backend
>>>      Chart.yaml
>>>>         templates
>>>>>           frontend-configMap.yaml
>>>>>           frontend-service.yaml
>>>>>           frontend.yaml
>>>>>           ingress.yaml

repeat the workflow with the following Chart.yaml:
Chart.yaml for database:  
```
apiVersion: v2
appVersion: "3.6"
description: A Helm chart for Guestbook Database Mongodb 3.6 
name: database
version: 0.1.0
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

so you should have the following structure:  

> guestbook
>>   Chart.yaml
>>   templates
>>>      backend
>>>      Chart.yaml
>>>>         templates
>>>>>           backend-secret.yaml
>>>>>           backend-service.yaml
>>>>>           backend.yaml
>>>      database
>>>      Chart.yaml
>>>>         templates
>>>>>           mongodb-persistent-volume-claim.yaml
>>>>>           mongodb-persistent-volume.yaml
>>>>>           mongodb-secret.yaml
>>>>>           mongodb-service.yaml
>>>>>           mongodb-yaml
>>>      frontend
>>>      Chart.yaml
>>>>         templates
>>>>>           frontend-configMap.yaml
>>>>>           frontend-service.yaml
>>>>>           frontend.yaml
>>>>>           ingress.yaml
         

---

### <font color='red'> 2.1.2 Deploy with Helm Chart </font>
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

if you problems resolving the URL you may need to update the /etc/hosts with frontend POD IP

to delete all revisions:
```
helm uninstall guestbook-demo
```

---