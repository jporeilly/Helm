## <font color='red'>3.2 Helm Template logic</font>
In these Labs you're going to cover:
* Build a Helm Template
* Release Guestbook v2

* Uninstall


---

### <font color='red'>Helm Template - _helper.tplFrontend</font>
our Helm template is nearly there, but there's still room for improvement..
start with frontend
* frontend-configmap.yaml
* frontend-service.yaml
* frontend.yaml
* ingress.yaml

lets take a look at the directory structure:
```
tree
```
* open the frontend.yaml.
* notice directives are repeated.

to make the name: frontend-config unique (in case there are several releases):
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ .Release.Name }}-{{ .Chart.Name }}
```
so if the value changed, you would have to edit in quite a few places..

let's move the code into a _helper.tpl file:
```
touch guestbook v2 - start/charts/frontend/templates/_helper.tpl
```
add the following:
```
{{- define "frontend.fullname" -}}
{{- if .Values.fullnameOverride -}}
{{- .Values.fullnameOverride | trunc 63 | trimSuffix "-" -}}
{{- else -}}
{{- printf "%s-%s" .Release.Name .Chart.Name | trunc 63 | trimSuffix "-" -}}
{{- end -}}
{{- end -}}
```
Note: so its now wrapped up in a define directive.
replace all the directives in all the frontend manifests:
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ include "frontend.fullname" . }}
...
```
Note: don't forget to set the scope with .
replace all the directives in all the frontend manifests:
* frontend-configmap.yaml
* frontend-service.yaml
* frontend.yaml
* ingress.yaml

so repeat the process for the backend..


let's move the code into a _helper.tpl file:
```
touch guestbook v2 - start/charts/backend/templates/_helper.tpl
```
add the following:
```
{{- define "backend.fullname" -}}
{{- if .Values.fullnameOverride -}}
{{- .Values.fullnameOverride | trunc 63 | trimSuffix "-" -}}
{{- else -}}
{{- printf "%s-%s" .Release.Name .Chart.Name | trunc 63 | trimSuffix "-" -}}
{{- end -}}
{{- end -}}
```
replace all the directives in all the frontend manifests:
* backend-secret.yaml
* backend-service.yaml
* backend.yaml
* ingress.yaml

---

### <font color='red'>Fix Database bug</font>
Time to fix the database bug.. 
the reason why it failed is because the backend services name depends on the {{ .Values.secrets.mongodb_uri }}-secret
and its been hardcoded as: mongodb and also dynamically built hostname which is the {{ .Release.Name}}
```
data:
  mongodb-uri: bW9uZ29kYjovL2FkbWluOnBhc3N3b3JkQG1vbmdvZGI6MjcwMTcvZ3Vlc3Rib29rP2F1dGhTb3VyY2U9YWRtaW4=
#              "mongodb://admin:password@mongodb:27017/guestbook?authSource=admin"
```
So the URI needs to broken down..
in the backend/templates/values.yaml we need to list the parts that make up the connection string:
```
secret:
  mongodb_uri:
    username: your_db_username
    password: your_db_password
    dbchart: database
    dbport: 27017
    dbconn: "guestbook?authSource=admin"
...
```
then this list can be referenced in the backend-secret.yaml file:
``
apiVersion: v1
kind: Secret
metadata:
  name: {{ include "backend.fullname" . }}-secret 
data:
  mongodb-uri: {{ with .Values.secret.mongodb_uri -}}
  {{- list "mongodb://" .username ":" .password "@" $.Release.Name "-" .dbchart ":" .port "/" .dbconn | join ""  | b64enc |  quote }}
# {{- ( printf "%s%s:%s@%s-%s%s" "mongodb://" .username .password $.Release.Name "database" ":27017/guestbook?authSource=admin" ) | b64enc | quote }}
{{- end }}
```
Note: all the strings are joined, then encoded in Base64 and finally entered in quotes.

now the bug should be fixed ..  so lets test..


ensure that your integrated terminal is pointing to the 02 Template Logic directory..
check the template:
```
helm template guestbook | less
```
if everything is Ok:
```
helm upgrade demo-guestbook guestbook
```
check deployment:
```
kubectl get all
```
Note: everything should be OK..

  > open in browser: http://frontend.minikube.local
---