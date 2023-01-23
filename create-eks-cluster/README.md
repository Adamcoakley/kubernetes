# Elastic Kubernetes Service (EKS) 
EKS is a managed Kubernetes service that makes it easy for you to run Kubernetes on AWS. This service in AWS can be used to create, configure and manage a Kubernetes Cluster. In this tutorial, we will create an EKS Cluster and attach a nodegroup.

## Prerequisites 
* Create your IAM User & Roles, and ensure you have an EC2 Key-Pair.
* Install awscli and set up credentials using aws configure.
* Install eksctl.

## Commands Used
If everything is set up correctly, the following command will deploy an EKS Cluster in Ireland for you:
```
eksctl create cluster \
--name DemoCluster \
--region eu-west-1 \
--nodegroup-name DemoNodes \
--nodes 2 \
--nodes-min 2 \
--nodes-max 3 \
--node-type t3.micro \
--with-oidc \
--ssh-access \
--ssh-public-key YourKeyPairName \
--managed
```

You should now see something like this, which means you have successfully deployed an EKS Cluster:

<p align="center">
    <img src="https://github.com/Adamcoakley/kubernetes/blob/main/create-eks-cluster/eks-cluster.png?raw=true">
</p>

