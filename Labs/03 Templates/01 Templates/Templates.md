## <font color='red'>3.1 Helm Templates</font>
In these Labs you're going to cover:
* Build a Helm Template
* Release Guestbook v2

* Uninstall


#### <font color='red'>IMPORTANT:</font> 
<strong>Please ensure you start with a clean environment. 
If you have previously run minikube, in another course, you will need to delete the existing instance.</strong>

to delete  minikube:
```
minikube delete
```

to start minikube:
```
minikube start
```

minikube tunnel:
```
minikube tunnel
```

check minikube status:
```
minikube status
```

---

### <font color='red'> Helm Template - Frontend</font>
create a Helm Template to deploy Guestbook v2..
start with frontend
* frontend-confogmap.yaml
* frontend-service.yaml
* frontend.yaml
* ingress.yaml


* values.yaml

lets take a look at the directory structure:
```
tree
```
* open the frontend-configmap.yaml.
* notice values are hard-coded.

to make the name: frontend-config unique (in case there are several releases):
```
{{ .Release.Name}}-{{ .Chart.Name}}-config
```
Note: these values come from chart.yaml

next guestbook-name & backend-uri..

create a templates/values.yaml:
```
touch guestbook v2 - start/charts/frontend/values.yaml
```
add the following properties:
```
config:
  guestbook-name: "MyPopRock Festival 2.0"
  backend-uri: "http://backend.minikube.local/guestbook"
```
template doesnt support - so change:
```
config:
  guestbook_name: "MyPopRock Festival 2.0"
  backend_uri: "http://backend.minikube.local/guestbook"
```
back to frontend-config, and change values:
```
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-{{ .Chart.Name }}-config
data:
  guestbook-name: {{ .Values.config.guestbook_name }}
  backend-uri: {{ .Values.config.backend_uri }}
```
next lets tackle frontend:
* open the frontend.yaml.
* notice values are hard-coded.

to make the name: frontend unique:
```
{{ .Release.Name}}-{{ .Chart.Name}}
```
Note: these values come from chart.yaml
again replace wherever you see frontend (apart from container name):
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ .Release.Name}}-{{ .Chart.Name}}
spec:
  replicas: 1
  selector:
    matchLabels:
      app: {{ .Release.Name}}-{{ .Chart.Name}} 
  template:
    metadata:
      labels:
        app: {{ .Release.Name}}-{{ .Chart.Name}}
    spec:
      containers:
      - image: jporeilly/frontend:2.0
        imagePullPolicy: Always
        name: {{ .Release.Name}}-{{ .Chart.Name}}
        ports:
        - name: frontend
          containerPort: 4200
        env:
        - name: BACKEND_URI
          valueFrom:
            configMapKeyRef:
              name: {{ .Release.Name}}-{{ .Chart.Name}}-config
              key: backend-uri
        - name: GUESTBOOK_NAME
          valueFrom:
            configMapKeyRef:
              name: {{ .Release.Name}}-{{ .Chart.Name}}-config
              key: guestbook-name
```
if we want to scale the app, then replicas..
so in values.yaml add:
```
config:
  guestbook_name: "MyPopRock Festival 2.0"
  backend_uri: "http://backend.minikube.local/guestbook"
replicaCount: 1
```
so now we can add:
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ .Release.Name}}-{{ .Chart.Name}}
spec:
  replicas: {{ .Values.replicaCount }}
  selector:
    matchLabels:
      app: {{ .Release.Name}}-{{ .Chart.Name}} 
  template:
    metadata:
      labels:
        app: {{ .Release.Name}}-{{ .Chart.Name}}
    spec:
      containers:
      - image: jporeilly/frontend:2.0
        imagePullPolicy: Always
        name: {{ .Release.Name}}-{{ .Chart.Name}}
        ports:
        - name: frontend
          containerPort: 4200
        env:
        - name: BACKEND_URI
          valueFrom:
            configMapKeyRef:
              name: {{ .Release.Name}}-{{ .Chart.Name}}-config
              key: backend-uri
        - name: GUESTBOOK_NAME
          valueFrom:
            configMapKeyRef:
              name: {{ .Release.Name}}-{{ .Chart.Name}}-config
              key: guestbook-name
```
also would like to change the image if a new version is deployed:
```
    spec:
      containers:
      - image: jporeilly/frontend:2.0
        imagePullPolicy: Always
```
so back in our values.yaml:
```
config:
  guestbook_name: "MyPopRock Festival 2.0"
  backend_uri: "http://backend.minikube.local/guestbook"
replicaCount: 1
image:
  repository: jporeilly/frontend
  tag: "2.0"
```
so now we can add:
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ .Release.Name}}-{{ .Chart.Name}}
spec:
  replicas: {{ .Values.replicaCount }}
  selector:
    matchLabels:
      app: {{ .Release.Name}}-{{ .Chart.Name}} 
  template:
    metadata:
      labels:
        app: {{ .Release.Name}}-{{ .Chart.Name}}
    spec:
      containers:
      - image: {{ .Values.image.repository }}:{{ .Values.image.tag }}
        imagePullPolicy: Always
        name: {{ .Release.Name}}-{{ .Chart.Name}}
        ports:
        - name: frontend
          containerPort: 4200
        env:
        - name: BACKEND_URI
          valueFrom:
            configMapKeyRef:
              name: {{ .Release.Name}}-{{ .Chart.Name}}-config
              key: backend-uri
        - name: GUESTBOOK_NAME
          valueFrom:
            configMapKeyRef:
              name: {{ .Release.Name}}-{{ .Chart.Name}}-config
              key: guestbook-name
```
ok..  we're good to go..

lets take a look at service ..
* open the frontend-service.yaml.
* notice values are hard-coded.

easy to change frontend, but also want to change port:
so back in our values.yaml:
```
config:
  guestbook_name: "MyPopRock Festival 2.0"
  backend_uri: "http://backend.minikube.local/guestbook"
replicaCount: 1
image:
  repository: jporeilly/frontend
  tag: "2.0"
service:
  port: 80
  type: ClusterIP
```
so now we can add:
```
apiVersion: v1
kind: Service
metadata:
  labels:
    name: {{ .Release.Name }}-{{ .Chart.Name }}
  name: {{ .Release.Name }}-{{ .Chart.Name }}
spec:
  type: {{ .Values.service.type }}
  ports:
    - protocol: "TCP"
      port: {{ .Values.service.port }}
      targetPort: 4200
  selector:
    app: {{ .Release.Name }}-{{ .Chart.Name }}
```
last one..!  ingress
* open the ingress.yaml.
* notice values are hard-coded.
* one ingress for both front and backend - split

so lets change name:
```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: {{ .Release.Name }}-{{ .Chart.Name }}-ingress
spec:
  rules:
  - host: frontend.minikube.local
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: {{ .Release.Name }}-{{ .Chart.Name }}
            port:
              number: 80
```
lets change host:
so back in values.yaml:
```
config:
  guestbook_name: "MyPopRock Festival 2.0"
  backend_uri: "http://backend.minikube.local/guestbook"
replicaCount: 1
image:
  repository: jporeilly/frontend
  tag: "2.0"
service:
  port: 80
  type: ClusterIP
ingress:
  host: frontend.minikube.local
```
ok. the frontend is now complete..!  I'll leave you to figure out the backend and database ..

ensure that your integrated terminal is pointing to the 01 Templates directory..
check the template:
```
helm template guestbook | less
```
Note have look at the manifest thats going to be deployed.
another test is to do a dry-run:
```
helm install demo-guestbook guestbook --dry-run --debug
```
Note: the release name is now demo-guestbook.
if everything is Ok:
```
helm install demo-guestbook guestbook
```
check deployment:
```
kubectl get all
```
Note: there's an error with the backend..  any ideas?
Have a look at mongodb..