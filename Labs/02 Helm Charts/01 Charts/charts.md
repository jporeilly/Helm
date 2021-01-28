## <font color='red'> 2.1 Helm Charts </font>
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
apiVersion: v1
appVersion: "1.0"
description: A Helm chart for Guestbook 1.0 
name: guestbook
version: 0.1.0
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

guestbook
   Chart.yaml
   templates
      frontend-service.yaml
      frontend.yaml
      ingress.yaml
