### <font color='red'> 1.2.1 Deploy Guestbook App v1</font>
In the first phase on the deployment is just the frontend.

deploy the guestbook app v1:
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
check guestbook in browser:

> Guestbook: http://frontend.minikube.local/guestbook

if you cant resolve the URL then you may have to map the IP to the etc/hosts.

check Ingress IP address:
```
kubectl get ingress
```
take a note of the IP..

--- 

### <font color='red'> 1.2.1 Deploy Guestbook App v2</font>
In the second phase a backend and database was added. To successfully deploy the app the YAML files need to be 
executed in order.  To help a shell script has been created..  Now dev want to rollback to v1..  :)

deploy guestbook app v2:
```
./deploy_guestbook_v2.sh
```
check guestbook in browser:

> Guestbook: http://frontend.minikube.local/guestbook

delete guestbook app v2:
```
./delete_guestbook_v2.sh
```
check PODs & services:
```
kubectl get pods,svc
```

if you run into problems with permisions:
```
sudo chmod 777 ./deploy_guestbook_v2.sh
```

--- 