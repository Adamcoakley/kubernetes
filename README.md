# Kubernetes
Kubernetes (k8s) is an open-source container orchestration system for automating deployments of containerised applications. Kubernetes has many solutions for managing and scaling very large quantities of containers by grouping them into manageable units.

# Structure 
A Kubernetes system is known as a cluster. **Pods** are the smallest unit or object in Kubernetes. A single Pod consists of one or more containers. The most common model is to have one container per Pod. This is generally the desired configuration for a Pod, as one Pod will then perform a single process.

## Issues?
By default, when a Pod is created it is connected to the pod network and given a private IP address. This can cause issues when trying to get that Pod to communicate with other things, since Pods are ephemeral (they are often deleted and made again) each time they are created, they are given a new private IP address. A better way to communicate with a Pod is to attach it to a **Service.** Services are used to expose Pods to a other networks. Services also allow the ephemeral Pods to always be accessed from the same address.

![controllers](https://i.gyazo.com/b8b8f2ba93a948f1127479f167ef020b.png)


## Controllers
Kubernetes is designed to manage and deploy containers (as Pods) on a massive scale. As a result, one rarely creates and manages the deployment of an application using individual Pods. Controllers are used to manage Kubernetes objects at scale by defining a desired state for the system. A Controller will constantly check the current state of the system and automatically work to pull it into the desired state. This process of constant checking is known as a control loop, and allows our Kubernetes clusters to self-manage with minimal human oversight.

![controllers](https://i.imgur.com/XUCQ3A6.png)

## Nodes
Pods in a Kubernetes cluster are hosted on Nodes. A Node may be a physical or virtual machine and there may be any number of Nodes in a cluster.

## Tutorial 
Within this repository, I create my first cluster and a number of pods, services and controllers.
