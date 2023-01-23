# Pods 
Pods are simply the smallest unit of execution in Kubernetes, consisting of one or more containers, typically one. We only use one container per pod in order to reduce coupling.

# Tutorial 
This tutorial shows you how to create a Pod from a manifest and run some basic kubectl commands.

## Prerequisites 
This module assumes you have created a Kubernetes cluster, have kubectl installed and have configured it to connect to your cluster.

## Commands 
I started by creating a new directory called pods, switching into it, and then creating a nginx.yaml file.
```
mkdir pods && cd $_
touch nginx.yaml
```

The content of the nginx.yaml file is as follows:
```
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    app: nginx
spec:
  containers:
  - name: nginx
    image: nginx:alpine
    ports:
    - containerPort: 80
```

I created a pod by referencing this file: 
```
kubectl apply -f nginx.yaml
```

<p align="center">
    <img src="https://github.com/Adamcoakley/kubernetes/blob/main/pods/apply-command.png?raw=true">
</p>

List the Pods running the in cluster: 
```
kubectl get pods
```

<p align="center">
    <img src="https://github.com/Adamcoakley/kubernetes/blob/main/pods/get-command.png?raw=true">
</p>

The exec command can be used to access the NGINX container running in the Pod: 
```
kubectl exec -it nginx -- sh
```

Try running some commands to see information about the container, such as its hostname:

<p align="center">
    <img src="https://github.com/Adamcoakley/kubernetes/blob/main/pods/exec-command.png?raw=true">
</p>
