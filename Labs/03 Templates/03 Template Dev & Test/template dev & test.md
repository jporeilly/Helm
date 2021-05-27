## <font color='red'>3.3 Helm Template - Dev & Test</font>
So we're nearly there..  DevOps has asked if they could have Dev & Test versions deployed in the same namespace..
In these Labs you're going to cover:
* Edit HOSTS file
* Dynamically create the hostnames
* Add NOTES.txt

---

### <font color='red'>Dynamically define hostnames</font>
lets first ensure the HOSTS can resolve the URLs:
```
sudo nano /etc/hosts
```
add the following hostnames:
```
dev.frontend.minikube.local
test.frontend.minikube.local
dev.backend.minikube.local
test.backend.minikube.local
```
currently the frontend & backend URLs are hardcoded in the values.yaml..   

calls from the frontend trigger http requests to the backend, hence the separate ingress instances.

thats fine but now they need to be dynamically generated.

to switch on / off the ingress add the following directive to the frontend & backend ingress.yaml:
```
{{- if .Values.ingress.enabled }}
apiVersion: networking.k8s.io/v1
kind: Ingress
....
{{- end }}
```
next in the backend/values.yaml set the ingress enabled to true:
```
ingress:
  enabled: true
  host: backend.minikube.local
```
Note: ensures you get an ingress by default if the chart is a standalone.
repeat for the frontend.
however, the ingress needs to be disabled at the top level chart.

create a values.yaml:
```
touch values.yaml
```
add the following:
```
backend:
  secret:
    mongodb_uri:
      username: admin
      password: password
  ingress:
    enabled: false
frontend:
  ingress:
    enabled: false

ingress:
  hosts:
    - host:
        domain: frontend.minikube.local
        chart: frontend
    - host:
        domain: backend.minikube.local
        chart: backend
```
Note: so we've disabled the separate frontend and backend ingress, but added the hosts for the top level ingress.

create a top level templates directory for the ingress.yaml:
```
sudo mkdir guestbook/templates
```
create ingress.yaml
```
touch ingress.yaml
```
add the following:
```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: {{ .Release.Name }}-{{ .Chart.Name }}-ingress
spec:
  rules:
{{- range .Values.ingress.hosts }}
  - host: {{ $.Release.Name }}.{{ .host.domain }}
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: {{ $.Release.Name}}-{{ .host.chart }}
            port:
              number: 80
{{- end }}
```
Note: this will serve both the frontend and backend as the {{ - range .Values.ingress.hosts}} will loop through the hosts.
Also we can now set the {{ $.Release.Name }}.{{ .host.domain}} variable with dev or test and the hostname will be dynamically generated.

last but least..  lets add a NOTES.txt:
```
Congratulations ! You installed {{ .Chart.Name }} chart sucessfully.
Release name is {{ .Release.Name }}

You can access the Guestbook application at the following urls :
{{- range .Values.ingress.hosts }}
  http://{{ $.Release.Name }}.{{ .host.domain }}
{{- end }}
Have fun !
```

finally also increment the Chart(.yaml) version to: 1.2.2


were now ready for release..!

test the manifests:
```
helm template guestbook | less
```
Note: check the ingress is dynamically generating the hosts.
check whats installed:
```
helm list --short
```
uninstall demo-guestbook:
```
helm uninstall demo-guestbook
```
install a dev release:
```
helm install dev guestbook  --set frontend.config.guestbook_name=DEV
```
now install test release:
```
helm install test guestbook  --set frontend.config.guestbook_name=TEST
```
verify the deployment:
```
kubectl get all
```

  > dev guestbook: http://dev.frontend.minikube.local/  


  > test guestbook: http://test.frontend.minikube.local/ 


---