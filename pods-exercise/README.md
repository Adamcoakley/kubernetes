# Exercise
Create a Pod template for the mysql:5.7 image. You will need to set the MYSQL_ROOT_PASSWORD and MYSQL_DATABASE environment variables for the Pod to succeed. 

## Template 
To begin with, I create a yaml file called mysql.yaml. The content of the file is as follows:
```
apiVersion: v1
kind: Pod
metadata:
  name: mysql
  labels:
    app: mysql
spec:
  containers:
  - name: mysql
    image: mysql:5.7
    ports:
    - containerPort: 3306
    env:
    - name: MYSQL_ROOT_PASSWORD
      value: "SuperSecretPassword123!"
    - name: MYSQL_DATABASE
      value: "demo_db"
```

In future tutorials, we will be learning about secrets, but for now, the env variables and their values are visible. 

## Commands
I created a pod by referencing the mysql.yaml file: 
```
kubectl apply -f mysql.yaml
```

<p align="center">
    <img src="https://github.com/Adamcoakley/kubernetes/blob/main/pods-exercise/apply-command.png?raw=true">
</p>

List the Pods running the in cluster: 
```
kubectl get pods
```

<p align="center">
    <img src="https://github.com/Adamcoakley/kubernetes/blob/main/pods-exercise/get-command.png?raw=true">
</p>

The exec command can be used to access the mysql container running in the Pod: 
```
kubectl exec -it mysql -- sh
```
