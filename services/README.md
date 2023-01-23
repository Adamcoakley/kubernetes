# Services 
Services are required to expose your Pods to a network, be that within the cluster or to an external network, such as the internet. Any Pod that needs network connectivity must be associated with a Service. This tutorial shows you how to create two Pods with Services attached:
* A simple frontend Python application that shows a page with a red background that will be exposed to the internet with a LoadBalancer Service.
* A simple backend Python application that generates a random string of letters that will be exposed within the cluster with a ClusterIP Service.

## Backend 
The backend.yaml file: 
```
apiVersion: v1
kind: Service
metadata:
  name: backend
spec:
  type: ClusterIP     # set the type of Service
  selector:
    app: backend      # referencing the Pod's label
  ports:
  - protocol: TCP
    port: 5001
    targetPort: 5001
---
apiVersion: v1
kind: Pod
metadata:
  name: backend
  labels:
    app: backend      # needs to be the same as Service's selector
spec:
  containers:
  - name: backend
    image: htrvolker/python-backend:letters
    ports:
    - containerPort: 5001
```

Above we are defining two manifests: one for the ClusterIP Service, and one for our backend Python server, which is running as a single Pod. We can then apply these manifests with:
```
kubectl apply -f backend.yaml
```

<p align="center">
    <img src="https://github.com/Adamcoakley/kubernetes/blob/main/services/apply-backend.png?raw=true">
</p>

## Frontend 
The frontend.yaml file: 
```
apiVersion: v1
kind: Service
metadata:
  name: frontend
spec:
  type: LoadBalancer  # set the type of Service
  selector:
    app: frontend     # referencing the Pod's label
  ports:
  - protocol: TCP
    port: 5000
    targetPort: 5000
---
apiVersion: v1
kind: Pod
metadata:
  name: frontend
  labels:
    app: frontend     # needs to be the same as Service's selector
spec:
  containers:
  - name: frontend
    image: htrvolker/python-frontend:red
    ports:
    - containerPort: 5000
```

Above we are defining two manifests: one for the LoadBalancer Service, and one for our frontend Python server, which is running as a single Pod. We can then apply these manifests with:
```
kubectl apply -f frontend.yaml
```

<p align="center">
    <img src="https://github.com/Adamcoakley/kubernetes/blob/main/services/apply-frontend.png?raw=true">
</p>

## Outcome
To find the external IP address of the LoadBalancer Service, run: 
```
kubectl get services
```
Once you have the IP address, copy it into your browser and access the application on port 5000. You should see a page that looks like this:

<p align="center">
    <img src="https://github.com/Adamcoakley/kubernetes/blob/main/services/browser.png?raw=true">
</p>
