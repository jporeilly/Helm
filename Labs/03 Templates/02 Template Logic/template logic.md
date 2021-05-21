## <font color='red'>3.2 Helm Template logic</font>
In these Labs you're going to cover:
* Build a Helm Template
* Release Guestbook v2

* Uninstall


---

#### <font color='red'>Helm Template - _helper.tplFrontend</font>
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