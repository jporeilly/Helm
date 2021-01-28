## <font color='red'> 2.1 Helm Charts </font>
in this Lab you will build a Guestbook Helm Chart.  
Ensure you're in the correct directory.

create a guestbook directory:
```
mkdir guestbook
```
change to guestbook directory:
```
cd guestbook
```
add a Chart.yaml file:
```
nano Chart.yaml
```
add the following:
```
apiVersion: v1
appVersion: "1.0"
description: A Helm chart for Guestbook 1.0 
name: guestbook
version: 0.1.0
```