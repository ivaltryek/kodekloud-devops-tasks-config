# Create Pods in Kubernetes Cluster

## Task Description 📔

The Nautilus DevOps team has started practicing some pods and services deployment on Kubernetes platform as they are planning to migrate most of their applications on Kubernetes platform. Recently one of the team members has been assigned a task to create a pod as per details mentioned below:

Create a pod named `pod-httpd` using httpd image with latest tag only and remember to mention the tag i.e `httpd:lates`t.

Labels app should be set to `httpd_app`, also container should be named as `httpd-container`.

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.

## Solution

- Create `pod.yaml` file
  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    labels:
      app: httpd_app
    name: pod-httpd
  spec:
    containers:
    - image: httpd:latest
      name: httpd-container
      ports:
      - containerPort: 80
      resources: {}
    dnsPolicy: ClusterFirst
    restartPolicy: Always
  ```

- Apply the file
  ```bash
  kubectl apply -f pod.yaml
  ```