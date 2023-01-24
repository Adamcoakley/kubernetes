# Overview 
A Controller is a Kubernetes object that constantly monitors and regulates the state of a system. There are many built-in Controllers for Kubernetes that can be used to automatically manage large deployments of Pods and other Kubernetes resources.

# Controllers 
Controllers get their name from the concept of a control loop – a non-terminating looping process that monitors and regulates a system. A control loop regulates a system based on the desired state, constantly checking the current state, comparing it to the desired state and automatically making changes to the system to bring it in line with the desired state. The desired state provided to a Kubernetes Controller is specified in its manifest. 

# Tutorial 
This tutorial shows you how to use a Deployment Controller to deploy a containerised application with multiple replicas, perform a rolling update and finally scale the Pods in each Deployment. The application we are going to deploy is a simple Python server with a 'frontend' and 'backend', where the frontend sends an HTTP request to the backend to retrieve a piece of information (a random number or string of letters).

# Commands 
Create a folder for this tutorial and create a YAML file for our app's manifest:
```
mkdir controllers && cd $_
touch frontend.yaml backend.yaml
```

## Backend
The backend.yaml file: 
```
apiVersion: v1
kind: Service
metadata:
  name: backend
spec:
  type: ClusterIP
  selector:
    app: backend
  ports:
  - protocol: TCP
    port: 5001
    targetPort: 5001
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: backend
  labels:
    app: backend
spec:
  replicas: 3
  selector:
    matchLabels:
      app: backend
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
  template:
    metadata:
      labels:
        app: backend
    spec:
      containers:
      - name: backend
        image: htrvolker/python-backend:letters
        ports:
        - containerPort: 5001
```

We can then apply these manifests with:
```
kubectl apply -f backend.yaml
```

<p align="center">
    <img src="https://github.com/Adamcoakley/kubernetes/blob/main/controllers/apply-backend.png?raw=true">
</p>

## Frontend 
The frontend.yaml file: 
```
apiVersion: v1
kind: Service
metadata:
  name: frontend
spec:
  type: LoadBalancer
  selector:
    app: frontend
  ports:
  - protocol: TCP
    port: 5000
    targetPort: 5000
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend
  labels:
    app: frontend
spec:
  replicas: 3
  selector:
    matchLabels:
      app: frontend
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
  template:
    metadata:
      labels:
        app: frontend
    spec:
      containers:
      - name: frontend
        image: htrvolker/python-frontend:red
        ports:
        - containerPort: 5000
```

We can then apply these manifests with:
```
kubectl apply -f frontend.yaml
```

<p align="center">
    <img src="https://github.com/Adamcoakley/kubernetes/blob/main/controllers/apply-frontend.png?raw=true">
</p>

## Outcome
To find the external IP address of the LoadBalancer Service associated with the frontend Deployment, run:
```
kubectl get services
```

<p align="center">
    <img src="https://github.com/Adamcoakley/kubernetes/blob/main/controllers/get-services.png?raw=true">
</p>

Once you have the IP address, copy it into your browser and access the application on port 5000. You should see a page that looks like this:

<p align="center">
    <img src="https://github.com/Adamcoakley/kubernetes/blob/main/controllers/red-browser.png?raw=true">
</p>

## Rolling Update
We're going to make two changes to our two-service application:
* We're going to change the background colour to blue.
* We're going to change the generated information to show numbers instead of letters.

I made a change to the manifest, so the frontend.yaml file looks as follows:
```
apiVersion: v1
kind: Service
metadata:
  name: frontend
spec:
  type: LoadBalancer
  selector:
    app: frontend
  ports:
  - protocol: TCP
    port: 5000
    targetPort: 5000
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend
  labels:
    app: frontend
spec:
  replicas: 3
  selector:
    matchLabels:
      app: frontend
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
  template:
    metadata:
      labels:
        app: frontend
    spec:
      containers:
      - name: frontend
        image: htrvolker/python-frontend:blue # this image will show a blue background instead of red
        ports:
        - containerPort: 5000
```

Now, we can re-apply the manifest:
```
kubectl apply -f frontend.yaml
```

<p align="center">
    <img src="https://github.com/Adamcoakley/kubernetes/blob/main/controllers/apply-frontend.png?raw=true">
</p>

The only change we've made is to the container's image. The new version of the application will show a blue background rather than the red, like so:

<p align="center">
    <img src="https://github.com/Adamcoakley/kubernetes/blob/main/controllers/blue-browser.png?raw=true">
</p>

