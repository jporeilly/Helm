### <font color='red'> 1.2.1 Deploy Guestbook App v1</font>
In the first phase on the deployment is just the frontend.

deploy the guestbook app:
```
kubectl apply -f 01_frontend.yaml
```
deploy service:
```
kubectl apply -f 02_frontend-service.yaml
```
deploy ingress:
```
kubectl apply -f 03_ingress.yaml
```
check PODs & services:
```
kubectl get pods,svc
```

--- 


### <font color='red'> 1.2.1 Deploy Guestbook App v2</font>