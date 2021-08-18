# Task Description 📓

The Nautilus DevOps team has started practicing some pods, and services deployment on Kubernetes platform, as they are planning to migrate most of their applications on Kubernetes. Recently one of the team members has been assigned a task to create a deploymnt as per details mentioned below:

Create a deployment named **nginx** to deploy the application **nginx** using the image **nginx:latest** (remember to mention the tag as well)

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.

## Solution

```console
thor@jump_host ~$ kubectl create deploy nginx --image=nginx:latest --dry-run=client -o yaml > deployment.yaml
thor@jump_host ~$ cat deployment.yaml 
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: nginx
  name: nginx
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: nginx
    spec:
      containers:
      - image: nginx:latest
        name: nginx
        resources: {}
status: {}

thor@jump_host ~$ kubectl create -f deployment.yaml 
deployment.apps/nginx created

thor@jump_host ~$ kubectl get po
NAME                     READY   STATUS    RESTARTS   AGE
nginx-55649fd747-5l5lh   1/1     Running   0          39s

thor@jump_host ~$ kubectl get deploy 
NAME    READY   UP-TO-DATE   AVAILABLE   AGE
nginx   1/1     1            1           45s

```

Click on **submit** to complete the task.