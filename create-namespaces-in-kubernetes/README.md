# Task Description 📔

The Nautilus DevOps team is planning to deploy some micro services on Kubernetes platform. The team has already set up a Kubernetes cluster and now they want set up some namespaces, deployments etc. Based on the current requirements, the team has shared some details as below:



Create a namespace named **dev** and create a **POD** under it; name the pod **dev-nginx-pod** and use nginx image with latest tag only and remember to mention tag i.e **nginx:latest**.

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.

---
```console
thor@jump_host ~$ kubectl config get-contexts
CURRENT   NAME             CLUSTER          AUTHINFO         NAMESPACE
*         kind-kodekloud   kind-kodekloud   kind-kodekloud

thor@jump_host ~$ kubectl get ns
NAME                 STATUS   AGE
default              Active   48m
kube-node-lease      Active   48m
kube-public          Active   48m
kube-system          Active   48m
local-path-storage   Active   48m

thor@jump_host ~$ kubectl create ns 
devnamespace/dev created

thor@jump_host ~$ kubectl get ns
NAME                 STATUS   AGE
default              Active   48m
dev                  Active   2s
kube-node-lease      Active   49m
kube-public          Active   49m
kube-system          Active   49m
local-path-storage   Active   48m


thor@jump_host ~$ kubectl run dev-nginx-pod --image=nginx:latest -n dev
pod/dev-nginx-pod created

thor@jump_host ~$ kubectl get po -n dev
NAME            READY   STATUS              RESTARTS   AGE
dev-nginx-pod   0/1     ContainerCreating   0          10s
thor@jump_host ~$ kubectl get po -n dev -owide
NAME            READY   STATUS    RESTARTS   AGE   IP           NODE                      NOMINATED NODE   READINESS GATES
dev-nginx-pod   1/1     Running   0          18s   10.244.0.6   kodekloud-control-plane   <none>           <none>

```